---
# the default layout is 'page'
image:
  path: assets/image/photo-1485827404703-89b55fcc595e.jpg
  alt: 'mega'
icon: fas fa-book-circle
tags: [automation, python]
---

# Automating invoice creation

This entry is about my application of programming to automate an important albeit time-consuming project management task: creating invoices.

### Problem Statement

At the end of each month I will need to create invoices for 20+ projects. Granularity of the invoice is broken down at ticket, category, and project levels. This is how this task was being done. Data were exported from a time tracking app to Google Sheets where pivot table was used to aggregate the numbers. Next, I would have to type each ticket number in an invoice card, and copy and paste the hours and fee accordingly. It has been time-consuming, and not to mention it is inductive to human errors during the copy and paste exercise.

In the beginning, automating this task was almost impossible because category was not standardized across projects, and there were too many data entry errors or missing data. It took a month or two to implement and enforce good data entry practices among our team as well as standardize the categories across all projects. It was the first crucial step that makes this automation possible.

Given the multi-hierarchy aggregation requirement, it is too cumbersome to use Pandas even though the `groupby` function in Pandas does support multi-hierarchy aggregation to some extent. Instead, I’ve utilized Python to aggregate the hours and cost at ticket, category and project levels.

### Understanding the Data

The screenshot below is a snapshot of the data input. In this data sample there are seven columns and 232 rows. Each row is one time entry that records the hours and billable amount for a particular task done for a particular ticket that belongs to a particular project during a period of time. For instance, the first entry records 1.25 hours spent on doing quality assurance testing for ticket 52050 of project Neeko on 2023-11-29, and this entry can be billed for $218.75. One ticket can have several entries (rows).

![](/assets/image/Screen_Shot_2023-12-29_at_4.00.59_PM.png)

`Task_name` column is indeed the task category with the following values: Development, Quality assurance, Cloud Admin Support, Research/ Investigation, Design, Project management, Client meetings. This classification is helpful for internal review of team utilization but can be a little too much for our clients. Therefore some modifications are needed. Specifically,

The time spent on `Development` and `Quality assurance` per ticket is put together, and goes under the category of `Development`.

Project management and Client meetings don’t need breaking down to ticket level.

Below is the invoice sample output. It shows how an invoice for one project should look like.

```
✅ Invoice sample
Teemo

Client meetings: 8.25 hours - $1,443.75

Cloud Admin Support:

- Ticket 75925: 4.0 hours - $700.0

- Ticket 77706: 2.0 hours - $350.0

- Ticket 95361: 1.5 hours - $262.5

Project management: 6.25 hours - $1,093.75

Research/ Investigation:

Ticket 18009: 2.75 hours - $481.25

- Ticket 75925: 8.0 hours - $1,400.0

- Ticket 71593: 0.5 hours - $87.5

- Ticket 77706: 1.0 hours - $175.0

Development:

- Ticket 75925: 9.5 hours - $1,662.5

- Ticket 71593: 0.5 hours - $87.5

Total: 44.25 hours - $7,743.75
```

### Code Explanation

The codebase and data sample can be found [here](https://gist.github.com/zoedieptran/8de6750c1d90508919ab313626ddd4cc).

The code follows a step-by-step approach, starting with reading the data, cleaning it, and then processing and aggregating the information at different levels (ticket, category, project).

The code is organized into functions, each serving a specific purpose. This modular design makes the code easier to understand and maintain. Each function has a clear responsibility, such as grouping data, aggregating totals, or formatting strings.

### Impact

This automation is a huge help. I’ve been able to cut down the time spent on creating invoices from a few hours to less than half an hour (I know!), and minimize the errors during copy and paste.
