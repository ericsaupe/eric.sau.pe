---
templateKey: blog-post
title: MySQL Queries for Grouping Data By Year
date: '2013-09-19T14:49:00-07:00'
tags:
  - django
  - django cursor
  - django mysql
  - mysql
  - mysql annual query
---
A big request we had in designing our new system was an easier way for users to get data from the database.  With the large amounts of data that we use across dozens of tables it can be slow going to use the Django methods.  Instead they have given the ability to use and write raw SQL and use that result how you please.  You can read about using cursors <a title="Django Cursors" href="https://docs.djangoproject.com/en/dev/topics/db/sql/#connections-and-cursors" target="_blank">here</a>, I may do a post in the future about them but today I want to focus on the SQL we wrote and the problems we encountered.

The requested query was to get a list of customers, how much business we did with them, and break that up by year.  The year could be inputted by the user so it needed to be dynamic enough to scale.  The first way we tried was to create a temporary table for every year needed.  Each table was created from a <code>SELECT</code> statement that was specified for the office as well as the year.  Below is the SQL command for one of the offices and years but you can imagine we copied this code, almost verbatim, but changed the <code>SELECT</code> statement to get the correct data.
<pre><code>DROP TABLE IF EXISTS main_office_%s;
CREATE TEMPORARY TABLE main_office_%s
SELECT
  company
  , COUNT(*) AS count
  , client_id
FROM
  WorkOrders
WHERE
  (YEAR(DateArrived) = YEAR(DATE_SUB(CURDATE(), INTERVAL %s YEAR)))
  AND
  office = 'Main'
GROUP BY
  client_id
ORDER BY
  COUNT(*) DESC
;

...Other Office Tables and Queries...

SELECT
  main_office_%s.name AS name
  , main_office_%s.count AS count
  , slc_office_%s.count AS slc_office_%s_count
  , ch_office_%s.count AS ch_office_%s_count
  , xs_office_%s.count AS xs_office_%s_count
  , tp_office_%s.count AS tp_office_%s_count
  , main_office_%s.client_id AS client_id
FROM
  main_office_%s
LEFT JOIN slc_office_%s
  ON main_office_%s.client_id = slc_office_%s.client_id
LEFT JOIN ch_office_%s
  ON main_office_%s.client_id = ch_office_%s.client_id
LEFT JOIN xs_office_%s
  ON main_office_%s.client_id = xs_office_%s.client_id
LEFT JOIN tp_office_%s
  ON main_office_%s.client_id = tp_office_%s.client_id
ORDER BY
  main_office_%s.count DESC
;</code></pre>
Once all of these tables were created we had to do a ton of <code>LEFT JOIN</code>s against the main office to get the split of the total between the other offices.  Needless to say this ran <em>very</em> slowly.  Using Python to change the <code>%s</code> to a year a typical loop through creating the tables, gathering the data, and returning took roughly 16 seconds.  Yeah, not good.  The good part was that it automatically put everything into one line for each client and one column for every office total and annual total.

Obviously we had to cut down on the time.  By changing how we did our whole process we can cut the time down significantly without using <code>JOIN</code>s.  Now this may not work in perfectly raw SQL because we are saving the output to a variable and then combining them.  Below is the new SQL code again changing the first one for each office.  The difference here is that instead of doing a table and <code>SELECT</code> for every year we are doing one <code>SELECT</code> for all the years and creating a row for each annual total.  So requesting data 5 years back could give a possible five rows for each customer, one for every year.
<pre><code>SELECT
  company
  , COUNT(*) AS count
  , client_id
  , YEAR(DateArrived) AS year
FROM
  WorkOrders
WHERE
  (YEAR(DateArrived) BETWEEN YEAR(DATE_SUB(CURDATE(), INTERVAL %s YEAR)) AND YEAR(CURDATE()))
AND
  office = 'Main'
GROUP BY
  client_id
  , YEAR(DateArrived)
ORDER BY
  COUNT(*) DESC
;

...Other Office Queries...</code></pre>
After every select we save the output to a variable named for the office.  In Python these are saved as lists of dictionaries.  In the end we combine the lists and then create an output of the combined totals.  I wrote an algorithm to loop through the giant list and adding the data to an output dictionary that had only one entry for each client.
<pre><code>all_data = all_data + slc_data + ch_data + xs_data + tp_data
    output = SortedDict()
    for company in all_data:
        #Set variables for easy access
        name = company['name']
        year = company['year']

        #Figure out office prefix
        prefix = ''
        if 'slc_count' in company:
            prefix = 'slc_'
        elif 'ch_count' in company:
            prefix = 'ch_'
        elif 'xs_count' in company:
            prefix = 'xs_'
        elif 'tp_count' in company:
            prefix = 'tp_'

        #Get count and add company to output if not there already
        count = company['%scount' % prefix]
        if not name in output:
            client_id = company['client_id']
            output[name] = {'client_id':client_id}

        #Add data to output company
        output[name]['%scount_%s' % (prefix, year)] = company['%scount' % prefix]</code></pre>
By changing the way that we create the tables we were able to cut that time to ~5 seconds.  It's a lot of data so it takes some time but 1/3 the last time is a big improvement. Obviously this code won't work in just SQL but it is useful for Django cursors.  If there is a way to accomplish what we are doing, in a <em>very fast</em> query I would totally be up for hearing about it in the comments.  I have altered our actual MySQL statements to be more readable.  In the end we just wanted a list of clients and the total work orders done for a year as well as the total for each office for that year.  Our solution works and we are fairly happy with it.  Always room for improvement though.
