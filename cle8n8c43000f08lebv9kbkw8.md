---
title: "Time Table just using HTML..."
seoTitle: "Time table project just using HTML"
datePublished: 2023-02-17T14:45:48.243Z
cuid: cle8n8c43000f08lebv9kbkw8
slug: time-table-just-using-html--deleted
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/hpjSkU2UYSU/upload/729c1356f12e71e7df639a4707b3c85c.jpeg
tags: html, html5, table

---

So in this blog, I will explain to you how you can create a <mark> timetable</mark> just using HTML. I know it's not the kind of project that very few people will be able to complete, but as a beginner, it will give you an idea about the working of tables in HTML...

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>TIME TABLE</title>
  </head>
  <body>
    <table border="2px" cellpadding="8px" cellspacing="2px">
      <!-- project for creating a time table just y=using HTML -->
      <thead>
        <tr>
          <th colspan="8">TIME TABLE</th>
        </tr>
        <tr>
          <th>Day</th>
          <th>Lecture 1 <br />8:00-8:30</th>
          <th>Lecture 2 <br />8:30-9:00</th>
          <th>Lecture 3 <br />9:00-9:30</th>
          <th>Lunch <br />9:30-10:30</th>
          <th>Lecture 4 <br />10:30-11:00</th>
          <th>Lecture 5 <br />11:00-11:30</th>
          <th>Lecture 6 <br />11:30-12:00</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>Monday</td>
          <td>Maths</td>
          <td>Dbms</td>
          <td>Automata</td>
          <td rowspan="6"></td>
          <td>Hindi</td>
          <td>English</td>
          <td>Science</td>
        </tr>
        <tr>
          <td>Tuesday</td>
          <td>Maths</td>
          <td>Dbms</td>
          <td>Automata</td>
          <!-- if we have written colespan then we dont need to write  that row   -->
          <td>Hindi</td>
          <td>English</td>
          <td>Science</td>
        </tr>
        <tr>
          <td>Wednesday</td>
          <td>Maths</td>
          <td>Dbms</td>
          <td>Automata</td>
          <td>Hindi</td>
          <td>English</td>
          <td>Science</td>
        </tr>
        <tr>
          <td>Thrusday</td>
          <td>Maths</td>
          <td>Dbms</td>
          <td>Automata</td>
          <td>Hindi</td>
          <td>English</td>
          <td>Science</td>
        </tr>
        <tr>
          <td>Friday</td>
          <td>Maths</td>
          <td>Dbms</td>
          <td>Automata</td>
          <td>Hindi</td>
          <td>English</td>
          <td>Science</td>
        </tr>
        <tr>
          <td>Saturday</td>
          <td>Maths</td>
          <td>Dbms</td>
          <td>Automata</td>
          <td>Hindi</td>
          <td>English</td>
          <td>Science</td>
        </tr>
      </tbody>
    </table>
  </body>
</html>
```

So this is the code now I am gonna explain to you it's working the code from `<Doctype HTML> to </head> is basically constructed with the help of emmet to know what that code means you can read my old blog in which I have explained all those lines of codes and their functions`. In this, we will just focus on the body and especially on the ***table*** tag.

So in HTML *table* is created using a table tag every single line of content written inside the opening and closing of the table tag will be stored in form of a table. Now with the latest update of HTML 5 a table has been divided into two sections ***table head and table body***.

So let's first talk about a <mark>table head</mark> that contains all the headings of a table for example if you see your school register so table head will contain sections like Name, Roll no., Father's name etc and so on. This table head contains `<tr>` tag that indicates that data will be stored in row-wise order. Then we have `<th>`tag that indicates that all the content inside these tags is table headings ...

Now table body contains the data associated with the table head for example your school register has your name under the name option and roll no. option. Table body too has `<tr>` tag to make sure that data is stored row-wise then it has `<td>` that indicates <mark>table data </mark> means the data we want to store inside the table...

EXTRA PROPERTIES USED -:

Although it's not good practice beautifying our content just using HTML but just to understand the work we are using these properties

1)Colspan -This property or attribute takes a number and occupies that much column space. In the above code, you can see that I have used colspan ="8" so it took space of 8 columns

2)cell spacing -This property or attribute is used to give space between adjacent cells so the table doesn't look compact and messy

3)Cell padding-This property or attribute is used to provide space between the contents and the border.

4)Rowspan -This property is used to provide the space or number of cells in rows of how much number is assigned to it to that particular content.

5)Border -This attribute or property is used to create the border around the content.

Hope you would understand the working of the table a bit ...In case of a query you can comment I am always there to help...

Thanks