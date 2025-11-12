
## The TMDb Database

In this series, we will be exploring [The Movie Database](https://www.themoviedb.org/) – an online movie and TV show database that houses some of the most popular movies and TV shows at your fingertips. The TMDb database supports 39 official languages used in over 180 countries daily and dates back all the way to 2008. 


<img src="https://github.com/Explore-AI/Pictures/blob/master/sql_tmdb.jpg?raw=true" width=80%/>


Below is an Entity Relationship Diagram (ERD) of the TMDb database:

<img src="https://github.com/Explore-AI/Pictures/blob/master/TMDB_ER_diagram.png?raw=true" width=70%/>

As can be seen from the ERD, the TMDb database consists of `12 tables` containing information about movies, cast, genre, and so much more.  

Let's get started!

## Loading the database

Before you begin, you need to prepare your SQL environment.  You can do this by loading the magic command `%load_ext sql`.


```python
# Load and activate the SQL extension to allow us to execute SQL in a Jupyter notebook. 
# If you get an error here, make sure that mysql and pymysql are installed correctly. 
# You can reload again the data.
%reload_ext sql
```

Next, go ahead and load your database. To do this, you will need to ensure you have downloaded the `TMDB.db` sqlite file and have stored it in a known location.


```python
%reload_ext sql
%sql sqlite:///TMDB-a-4006.db

```


```sql
%%sql
PRAGMA table_info(oscars);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>year</td>
            <td>varchar(10)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>1</td>
            <td>award</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2</td>
            <td>winner</td>
            <td>varchar(10)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>3</td>
            <td>name</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>4</td>
            <td>film</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
PRAGMA table_info(movies);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>movie_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>title</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2</td>
            <td>release_date</td>
            <td>datetime(6)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>3</td>
            <td>budget</td>
            <td>INTEGER</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>4</td>
            <td>homepage</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>5</td>
            <td>original_language</td>
            <td>varchar(50)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>6</td>
            <td>original_title</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>7</td>
            <td>overview</td>
            <td>varchar(5000)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>8</td>
            <td>popularity</td>
            <td>double</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>9</td>
            <td>revenue</td>
            <td>double</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>10</td>
            <td>runtime</td>
            <td>double</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>11</td>
            <td>release_status</td>
            <td>varchar(50)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>12</td>
            <td>tagline</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>13</td>
            <td>vote_average</td>
            <td>double</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>14</td>
            <td>vote_count</td>
            <td>INTEGER</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>



 1.What is the most recent movie Vin Diesel appeared in?


```sql
%%sql
SELECT a.actor_name,m.title,STRFTIME('%Y',m.release_date) AS year
FROM actors AS a
JOIN casts AS c
    ON c.actor_id=a.actor_id
JOIN movies AS m
    ON m.movie_id=c.movie_id
WHERE a.actor_name='Vin Diesel'
ORDER BY year DESC;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>actor_name</th>
            <th>title</th>
            <th>year</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Vin Diesel</td>
            <td>Furious 7</td>
            <td>2015</td>
        </tr>
        <tr>
            <td>Vin Diesel</td>
            <td>The Last Witch Hunter</td>
            <td>2015</td>
        </tr>
        <tr>
            <td>Vin Diesel</td>
            <td>Guardians of the Galaxy</td>
            <td>2014</td>
        </tr>
        <tr>
            <td>Vin Diesel</td>
            <td>Riddick</td>
            <td>2013</td>
        </tr>
        <tr>
            <td>Vin Diesel</td>
            <td>Fast Five</td>
            <td>2011</td>
        </tr>
        <tr>
            <td>Vin Diesel</td>
            <td>Babylon A.D.</td>
            <td>2008</td>
        </tr>
        <tr>
            <td>Vin Diesel</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
            <td>2006</td>
        </tr>
        <tr>
            <td>Vin Diesel</td>
            <td>Find Me Guilty</td>
            <td>2006</td>
        </tr>
        <tr>
            <td>Vin Diesel</td>
            <td>The Pacifier</td>
            <td>2005</td>
        </tr>
        <tr>
            <td>Vin Diesel</td>
            <td>The Chronicles of Riddick</td>
            <td>2004</td>
        </tr>
        <tr>
            <td>Vin Diesel</td>
            <td>A Man Apart</td>
            <td>2003</td>
        </tr>
        <tr>
            <td>Vin Diesel</td>
            <td>xXx</td>
            <td>2002</td>
        </tr>
        <tr>
            <td>Vin Diesel</td>
            <td>Knockaround Guys</td>
            <td>2001</td>
        </tr>
        <tr>
            <td>Vin Diesel</td>
            <td>The Fast and the Furious</td>
            <td>2001</td>
        </tr>
        <tr>
            <td>Vin Diesel</td>
            <td>Pitch Black</td>
            <td>2000</td>
        </tr>
        <tr>
            <td>Vin Diesel</td>
            <td>Boiler Room</td>
            <td>2000</td>
        </tr>
        <tr>
            <td>Vin Diesel</td>
            <td>The Iron Giant</td>
            <td>1999</td>
        </tr>
        <tr>
            <td>Vin Diesel</td>
            <td>Saving Private Ryan</td>
            <td>1998</td>
        </tr>
        <tr>
            <td>Vin Diesel</td>
            <td>The Fifth Element</td>
            <td>1997</td>
        </tr>
    </tbody>
</table>



List all other actors who have appeared in a movie with Vin Diesel. 


```sql
%%sql
SELECT a.actor_name,m.title
FROM actors AS a
JOIN casts AS c
    ON c.actor_id=a.actor_id
JOIN movies AS m
    ON m.movie_id=c.movie_id
WHERE c.movie_id IN (
    SELECT c.movie_id
    FROM actors AS a
    JOIN casts AS c
    ON c.actor_id=a.actor_id
    JOIN movies AS m
    ON m.movie_id=c.movie_id
    WHERE a.actor_name='Vin Diesel')
AND (a.actor_name !='Vin Diesel');
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>actor_name</th>
            <th>title</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Bruce Willis</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Milla Jovovich</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Gary Oldman</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Ian Holm</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Chris Tucker</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Brion James</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Lenny McLean</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Mathieu Kassovitz</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Lee Evans</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Luke Perry</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Tom Lister Jr.</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Charlie Creed-Miles</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>John Bluthal</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Christopher Fairbank</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Kim Chan</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Michael Culkin</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Al Matthews</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>John Neville</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Tim McMullan</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Richard Leaf</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Sam Douglas</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Fran+ºois Guillaume</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Julie T. Wallace</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Alan Ruscoe</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Mac McDonald</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>John Sharian</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Martin McDougall</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>David Barrass</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>John Bennett</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Jason Salkey</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>David Kennedy</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Christopher Adamson</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Ma+»wenn</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Marie Guillard</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Roger Monk</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Frank Senger</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Richard Ashton</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Tricky</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Robert Oates</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Sean Buckley</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Ali Yassine</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Indra Ov+¬</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Mia Frye</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Clifton Lloyd Bryan</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Cecil Cheng</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>+êve Salvail</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Sonita Henry</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Riz Meedin</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Hon Ping Tang</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Anthony Chinn</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Sonny Caldinez</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Jean-Luc Caron</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Carlton Chance</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Derek Ezenagu</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Bill Reimbold</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Gin Clarke</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Kevin Brewerton</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Anita Koh</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Joss Skottowe</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Zeta Graff</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Genevieve Maylam</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Renee Montemayor</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Stewart Harvey-Wilson</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>David Fishley</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Kristen Fick</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Pete Dunwell</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Ivan Heng</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Vincenzo Pellegrino</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>George Khan</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>John Hughes</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Roberto Bryce</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Said Talidi</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Justin Lee Burrows</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Jerome St. John Blake</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Kevin Molloy</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Colin Brooks</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Mark Seaton</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Jerry Ezekiel</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Rachel Willis</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Natasha Brice</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Sophia Goth</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Paul Priestley</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Vladimir McCrary</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Aron Paramor</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Kaleem Janjua</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Tyrone Tyrell</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Ian Beckett</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Eddie Ellwood</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Yui</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Laura De Palma</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Fred Williams</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Sibyl Buck</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Sarah Carrington</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Dane Messam</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Nathan Hamlett</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Scott Woods</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Leon Dekker</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>David Garvey</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Stanley Kowalski</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Omar Williams</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Robert Clapperton</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Robert Alexander</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Leo Williams</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>C. Keith Martin</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>J.D. Dawodu</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Patrick Nicholls</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Shaun Davis</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Roy Garcia</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Alex Georgijev</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Stina Richardson</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Kamay Lau</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Tracy Redington</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Gito Santana</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Roger Wright</td>
            <td>The Fifth Element</td>
        </tr>
        <tr>
            <td>Tom Hanks</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Dennis Farina</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Giovanni Ribisi</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Matt Damon</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Tom Sizemore</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Harve Presnell</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Jeremy Davies</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Adam Goldberg</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Edward Burns</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Barry Pepper</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Ted Danson</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Joerg Stadler</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Paul Giamatti</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Bryan Cranston</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Demetri Goritsas</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Leo Stransky</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Nathan Fillion</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Harrison Young</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Dylan Bruno</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Ian Porter</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Daniel Cerqueira</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>David Wohl</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Max Martini</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Kathleen Byron</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Julian Spencer</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>William Marsh</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Gary Sefton</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Anna Maguire</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Marc Cass</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Martin Hub</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Neil Finnighan</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Peter Miles</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Steve Griffin</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Markus Napier</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Paul Garcia</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Seamus McQuade</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Valerie Colgan</td>
            <td>Saving Private Ryan</td>
        </tr>
        <tr>
            <td>Cole Hauser</td>
            <td>Pitch Black</td>
        </tr>
        <tr>
            <td>Radha Mitchell</td>
            <td>Pitch Black</td>
        </tr>
        <tr>
            <td>Claudia Black</td>
            <td>Pitch Black</td>
        </tr>
        <tr>
            <td>Rhiana Griffith</td>
            <td>Pitch Black</td>
        </tr>
        <tr>
            <td>Lewis Fitz-Gerald</td>
            <td>Pitch Black</td>
        </tr>
        <tr>
            <td>Simon Burke</td>
            <td>Pitch Black</td>
        </tr>
        <tr>
            <td>Firass Dirani</td>
            <td>Pitch Black</td>
        </tr>
        <tr>
            <td>Keith David</td>
            <td>Pitch Black</td>
        </tr>
        <tr>
            <td>Les Chantery</td>
            <td>Pitch Black</td>
        </tr>
        <tr>
            <td>John Moore</td>
            <td>Pitch Black</td>
        </tr>
        <tr>
            <td>Sam Sari</td>
            <td>Pitch Black</td>
        </tr>
        <tr>
            <td>Ric Anderson</td>
            <td>Pitch Black</td>
        </tr>
        <tr>
            <td>Vic Wilson</td>
            <td>Pitch Black</td>
        </tr>
        <tr>
            <td>Angela Moore</td>
            <td>Pitch Black</td>
        </tr>
        <tr>
            <td>Karl Urban</td>
            <td>The Chronicles of Riddick</td>
        </tr>
        <tr>
            <td>Linus Roache</td>
            <td>The Chronicles of Riddick</td>
        </tr>
        <tr>
            <td>Judi Dench</td>
            <td>The Chronicles of Riddick</td>
        </tr>
        <tr>
            <td>Thandie Newton</td>
            <td>The Chronicles of Riddick</td>
        </tr>
        <tr>
            <td>Colm Feore</td>
            <td>The Chronicles of Riddick</td>
        </tr>
        <tr>
            <td>Terry Chen</td>
            <td>The Chronicles of Riddick</td>
        </tr>
        <tr>
            <td>Nick Chinlund</td>
            <td>The Chronicles of Riddick</td>
        </tr>
        <tr>
            <td>Roger Cross</td>
            <td>The Chronicles of Riddick</td>
        </tr>
        <tr>
            <td>Alexa Davalos</td>
            <td>The Chronicles of Riddick</td>
        </tr>
        <tr>
            <td>Christina Cox</td>
            <td>The Chronicles of Riddick</td>
        </tr>
        <tr>
            <td>Yorick van Wageningen</td>
            <td>The Chronicles of Riddick</td>
        </tr>
        <tr>
            <td>Keith David</td>
            <td>The Chronicles of Riddick</td>
        </tr>
        <tr>
            <td>Charles Zuckermann</td>
            <td>The Chronicles of Riddick</td>
        </tr>
        <tr>
            <td>Kim Hawthorne</td>
            <td>The Chronicles of Riddick</td>
        </tr>
        <tr>
            <td>Mark Gibbon</td>
            <td>The Chronicles of Riddick</td>
        </tr>
        <tr>
            <td>Alexis Llewellyn</td>
            <td>The Chronicles of Riddick</td>
        </tr>
        <tr>
            <td>Raoul Ganeev</td>
            <td>The Chronicles of Riddick</td>
        </tr>
        <tr>
            <td>Nigel Vonas</td>
            <td>The Chronicles of Riddick</td>
        </tr>
        <tr>
            <td>Samuel L. Jackson</td>
            <td>xXx</td>
        </tr>
        <tr>
            <td>Richy M++ller</td>
            <td>xXx</td>
        </tr>
        <tr>
            <td>William Hope</td>
            <td>xXx</td>
        </tr>
        <tr>
            <td>Danny Trejo</td>
            <td>xXx</td>
        </tr>
        <tr>
            <td>Asia Argento</td>
            <td>xXx</td>
        </tr>
        <tr>
            <td>Marton Csokas</td>
            <td>xXx</td>
        </tr>
        <tr>
            <td>Werner Daehn</td>
            <td>xXx</td>
        </tr>
        <tr>
            <td>Joe Bucaro III</td>
            <td>xXx</td>
        </tr>
        <tr>
            <td>Michael Roof</td>
            <td>xXx</td>
        </tr>
        <tr>
            <td>Petr J+íkl ml.</td>
            <td>xXx</td>
        </tr>
        <tr>
            <td>Thomas Ian Griffith</td>
            <td>xXx</td>
        </tr>
        <tr>
            <td>Leila Arcieri</td>
            <td>xXx</td>
        </tr>
        <tr>
            <td>Jan Pavel Filipensky</td>
            <td>xXx</td>
        </tr>
        <tr>
            <td>Tom Everett</td>
            <td>xXx</td>
        </tr>
        <tr>
            <td>Chris Gann</td>
            <td>xXx</td>
        </tr>
        <tr>
            <td>Eve</td>
            <td>xXx</td>
        </tr>
        <tr>
            <td>Ted Maynard</td>
            <td>xXx</td>
        </tr>
        <tr>
            <td>Martin Hub</td>
            <td>xXx</td>
        </tr>
        <tr>
            <td>David Asman</td>
            <td>xXx</td>
        </tr>
        <tr>
            <td>Marty Antonini</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Lawrence Bender</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Dennis Hopper</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Mike Starr</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>John Malkovich</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Barry Pepper</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Seth Green</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Kris Lemche</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Boyd Banks</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Shawn Doyle</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Arthur J. Nascarella</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Josh Mostel</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Kevin Gage</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Dov Tiefenbach</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Andy Davoli</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Nicholas Pasco</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Andrew Francis</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>John Liddle</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Allan Havey</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Tony Nappo</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Bruce McFee</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Joe Pingue</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Jennifer Baxter</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Tom Noonan</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Catherine Fitch</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>George Buza</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Michael A. Miranda</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Catherine Burdon</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Angela White</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Frank Pellegrino</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Ceciley Jenkins</td>
            <td>Knockaround Guys</td>
        </tr>
        <tr>
            <td>Juan Fern+índez</td>
            <td>A Man Apart</td>
        </tr>
        <tr>
            <td>Steve Eastin</td>
            <td>A Man Apart</td>
        </tr>
        <tr>
            <td>Timothy Olyphant</td>
            <td>A Man Apart</td>
        </tr>
        <tr>
            <td>Larenz Tate</td>
            <td>A Man Apart</td>
        </tr>
        <tr>
            <td>Jacqueline Obradors</td>
            <td>A Man Apart</td>
        </tr>
        <tr>
            <td>Emilio Rivera</td>
            <td>A Man Apart</td>
        </tr>
        <tr>
            <td>Geno Silva</td>
            <td>A Man Apart</td>
        </tr>
        <tr>
            <td>Jeff Kober</td>
            <td>A Man Apart</td>
        </tr>
        <tr>
            <td>Alice Amter</td>
            <td>A Man Apart</td>
        </tr>
        <tr>
            <td>Marco Rodr+¡guez</td>
            <td>A Man Apart</td>
        </tr>
        <tr>
            <td>Mike Moroff</td>
            <td>A Man Apart</td>
        </tr>
        <tr>
            <td>Malieek Straughter</td>
            <td>A Man Apart</td>
        </tr>
        <tr>
            <td>George Sharperson</td>
            <td>A Man Apart</td>
        </tr>
        <tr>
            <td>Jim Boeke</td>
            <td>A Man Apart</td>
        </tr>
        <tr>
            <td>Michelle Yeoh</td>
            <td>Babylon A.D.</td>
        </tr>
        <tr>
            <td>Lambert Wilson</td>
            <td>Babylon A.D.</td>
        </tr>
        <tr>
            <td>Mark Strong</td>
            <td>Babylon A.D.</td>
        </tr>
        <tr>
            <td>G+¬rard Depardieu</td>
            <td>Babylon A.D.</td>
        </tr>
        <tr>
            <td>Joel Kirby</td>
            <td>Babylon A.D.</td>
        </tr>
        <tr>
            <td>Jan Unger</td>
            <td>Babylon A.D.</td>
        </tr>
        <tr>
            <td>Curtis Matthew</td>
            <td>Babylon A.D.</td>
        </tr>
        <tr>
            <td>Charlotte Rampling</td>
            <td>Babylon A.D.</td>
        </tr>
        <tr>
            <td>J+¬r+¦me Le Banner</td>
            <td>Babylon A.D.</td>
        </tr>
        <tr>
            <td>M+¬lanie Thierry</td>
            <td>Babylon A.D.</td>
        </tr>
        <tr>
            <td>Radek Bruna</td>
            <td>Babylon A.D.</td>
        </tr>
        <tr>
            <td>David Belle</td>
            <td>Babylon A.D.</td>
        </tr>
        <tr>
            <td>Souleymane Dicko</td>
            <td>Babylon A.D.</td>
        </tr>
        <tr>
            <td>Abraham Belaga</td>
            <td>Babylon A.D.</td>
        </tr>
        <tr>
            <td>David Gasman</td>
            <td>Babylon A.D.</td>
        </tr>
        <tr>
            <td>Gary Cowan</td>
            <td>Babylon A.D.</td>
        </tr>
        <tr>
            <td>Lemmy Constantine</td>
            <td>Babylon A.D.</td>
        </tr>
        <tr>
            <td>Pete Thias</td>
            <td>Babylon A.D.</td>
        </tr>
        <tr>
            <td>Lucas Black</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Vincent Laresca</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Sonny Chiba</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Lynda Boyd</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Leonardo Nam</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Zachery Ty Bryan</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Shad Moss</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>David V. Thomas</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Trula M. Marcus</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Sung Kang</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Koji Kataoka</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Satoshi Tsumabuki</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Brian Goodman</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Tak Kubota</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Nikki Griffin</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Nathalie Kelley</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Brian Tee</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Keiko Kitagawa</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Kazutoshi Wadakura</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Jason Tobin</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Daniel Booko</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Kevin Ryan</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Silvia +áuvadov+í</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Kazuki Namioka</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Kaila Yu</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Yoko Maki</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Alden Ray</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Mitsuki Koga</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Sh+¦ko Nakagawa</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Rie Shibata</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Amber Stevens</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Aiko Tanaka</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Keiichi Tsuchiya</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Caroline de Souza Correa</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Brandon Brendel</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Britten Kelley</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Damien Marzett</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Ashika Gogna</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Christian Salazar</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Kevin Caira</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Julius Trey Sanford</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Danny Ray McDonald II</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Joseph Crumpton</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Toshi Hayama</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Atley Siauw</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Hiroshi Hatayama</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Konishiki</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Jimmy Lin</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Verena Mei</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Mari Jaramillo</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Mikiko Yano</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Wendy Watanabe</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Tina Tsunoda</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Stuart Yee</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Hidesuke Motoki</td>
            <td>The Fast and the Furious: Tokyo Drift</td>
        </tr>
        <tr>
            <td>Paul Walker</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Thom Barry</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Chad Lindberg</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Rick Yune</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Neal H. Moritz</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Johnny Strong</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Ted Levine</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Michelle Rodriguez</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Rob Cohen</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Reggie Lee</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Jordana Brewster</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Matt Schulze</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Noel Gugliemi</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Vyto Ruginis</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Ja Rule</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Kevin Smith</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>F. Valentino Morales</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Peter &#x27;Navy&#x27; Tuiasosopo</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>David Douglas</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Delphine Pacific</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Beau Holden</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Stanton Rutledge</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>RJ de Vera</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Doria Clare Anselmo</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Glenn K. Ota</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Mike White</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Monica Tamayo</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Megan Baker</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Tammy Monica Gegamian</td>
            <td>The Fast and the Furious</td>
        </tr>
        <tr>
            <td>Richard Portnow</td>
            <td>Find Me Guilty</td>
        </tr>
        <tr>
            <td>Frank Adonis</td>
            <td>Find Me Guilty</td>
        </tr>
        <tr>
            <td>Chuck Cooper</td>
            <td>Find Me Guilty</td>
        </tr>
        <tr>
            <td>Gerry Vichi</td>
            <td>Find Me Guilty</td>
        </tr>
        <tr>
            <td>Vinny Vella</td>
            <td>Find Me Guilty</td>
        </tr>
        <tr>
            <td>Paul Borghese</td>
            <td>Find Me Guilty</td>
        </tr>
        <tr>
            <td>Frank Pietrangolare</td>
            <td>Find Me Guilty</td>
        </tr>
        <tr>
            <td>Alex Rocco</td>
            <td>Find Me Guilty</td>
        </tr>
        <tr>
            <td>Peter Dinklage</td>
            <td>Find Me Guilty</td>
        </tr>
        <tr>
            <td>James Biberi</td>
            <td>Find Me Guilty</td>
        </tr>
        <tr>
            <td>Steven Randazzo</td>
            <td>Find Me Guilty</td>
        </tr>
        <tr>
            <td>Richard DeDomenico</td>
            <td>Find Me Guilty</td>
        </tr>
        <tr>
            <td>Jerry Grayson</td>
            <td>Find Me Guilty</td>
        </tr>
        <tr>
            <td>Tony Ray Rossi</td>
            <td>Find Me Guilty</td>
        </tr>
        <tr>
            <td>Nicholas A. Puccio</td>
            <td>Find Me Guilty</td>
        </tr>
        <tr>
            <td>Salvatore Paul Piro</td>
            <td>Find Me Guilty</td>
        </tr>
        <tr>
            <td>Frankie Perrone</td>
            <td>Find Me Guilty</td>
        </tr>
        <tr>
            <td>Oscar A. Colon</td>
            <td>Find Me Guilty</td>
        </tr>
        <tr>
            <td>Ben Lipitz</td>
            <td>Find Me Guilty</td>
        </tr>
        <tr>
            <td>Brad Garrett</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Kyle Schmid</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Shane Cardwell</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Hank Amos</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Carol Kane</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Tate Donovan</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Lauren Graham</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Adam Shankman</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Anne Fletcher</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Brittany Snow</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>David Sparrow</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Max Thieriot</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Faith Ford</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Chris Potter</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Scott Thompson</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>F. Valentino Morales</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>David Lipper</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Vanessa Cobham</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Alexander Conti</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Demetrius Joyette</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Morgan York</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Tig Fong</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Tommy Chang</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Denis Akiyama</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Scott Hurst</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Catherine Burdon</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Seth Howard</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Marcus Hutchings</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Cade Courtley</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Cassius Willis</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Jordan Allison</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Jordan Todosey</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Christian Laurin</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Charles Haugk</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Laura Jeanes</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Darryl Dinn</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Mung-Ling Tsui</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>John MacDonald</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Matt Purdy</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Curtis Parker</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Christopher Giroux</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Mary Pitt</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Toya Alexis</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Jean Pearson</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Gabriel Antonacci</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Allison Lynn</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Dan Sutcliffe</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Rachael Dolan</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Charlotte Szivak</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Valerie Boyle</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Karla Jang</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Silver Kim</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Robert Yeretch</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Christie Allaire</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Stephanie Allaire</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Amy Allicock</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Helena Chow</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Joella Crichton</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Rebecca Priestley</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Taryn Ash</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Maria Georgas</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Nikki Shah</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Emi Yaguchi-Chow</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Christopher Lortie</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Sotiri Georgas</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Steven Georgas</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Gannon Racki</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Evelyn Kaye</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Billy Oliver</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Robert Thomas</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Michael Quintero</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Cameron Hickox</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Jeff Podgurski</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Scott Reiff</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>Liam McGuckian</td>
            <td>The Pacifier</td>
        </tr>
        <tr>
            <td>M. Emmet Walsh</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>John Mahoney</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Christopher McDonald</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Jennifer Aniston</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Cloris Leachman</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Brian Tochi</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>James Gammon</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Harry Connick Jr.</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Jack Angel</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Mary Kay Bergman</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Robert Clotworthy</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Phil Proctor</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Frank Thomas</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Rodger Bumpass</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Bob Bergen</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Jennifer Darling</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Bill Farmer</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Mickie McGowan</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Paul Eiding</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Eli Marienthal</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Ollie Johnston</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Charles Howerton</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Ryan O&#x27;Donohue</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Sherry Lynn</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Michael Bird</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Devon Cole Borisoff</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Patti Tippo</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Zack Eginton</td>
            <td>The Iron Giant</td>
        </tr>
        <tr>
            <td>Ben Affleck</td>
            <td>Boiler Room</td>
        </tr>
        <tr>
            <td>Giovanni Ribisi</td>
            <td>Boiler Room</td>
        </tr>
        <tr>
            <td>Scott Caan</td>
            <td>Boiler Room</td>
        </tr>
        <tr>
            <td>Mark Webber</td>
            <td>Boiler Room</td>
        </tr>
        <tr>
            <td>Jamie Kennedy</td>
            <td>Boiler Room</td>
        </tr>
        <tr>
            <td>Siobhan Fallon</td>
            <td>Boiler Room</td>
        </tr>
        <tr>
            <td>Nia Long</td>
            <td>Boiler Room</td>
        </tr>
        <tr>
            <td>Ron Rifkin</td>
            <td>Boiler Room</td>
        </tr>
        <tr>
            <td>Tom Everett Scott</td>
            <td>Boiler Room</td>
        </tr>
        <tr>
            <td>Nicky Katt</td>
            <td>Boiler Room</td>
        </tr>
        <tr>
            <td>Kirk Acevedo</td>
            <td>Boiler Room</td>
        </tr>
        <tr>
            <td>Judy Del Giudice</td>
            <td>Boiler Room</td>
        </tr>
        <tr>
            <td>Paul Walker</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Tyrese Gibson</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Eva Mendes</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Ludacris</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Michelle Rodriguez</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Dwayne Johnson</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Yorgo Constantine</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Jordana Brewster</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Joaquim de Almeida</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Matt Schulze</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Luis Da Silva Jr.</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Esteban Cueto</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Sung Kang</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Elsa Pataky</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Kent Shocknek</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Mark Hicks</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Geoff Meed</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Gal Gadot</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Don Omar</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Tego Calder+¦n</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Michael Irby</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Corey Michael Eubanks</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Sharon Tay</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Alimi Ballard</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Carlos Sanchez</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Fernando Chien</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Luis Gonzaga</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Arlene Santana</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Joseph Melendez</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Pedro Garc+¡a</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Jeirmarie Osorio</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Benjamin Blankenship</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Arturo Gaskins</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Jay Jackson</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Andy Rosa Adler</td>
            <td>Fast Five</td>
        </tr>
        <tr>
            <td>Karl Urban</td>
            <td>Riddick</td>
        </tr>
        <tr>
            <td>Raoul Max Trujillo</td>
            <td>Riddick</td>
        </tr>
        <tr>
            <td>Conrad Pla</td>
            <td>Riddick</td>
        </tr>
        <tr>
            <td>Jordi Moll+á</td>
            <td>Riddick</td>
        </tr>
        <tr>
            <td>Katee Sackhoff</td>
            <td>Riddick</td>
        </tr>
        <tr>
            <td>Bokeem Woodbine</td>
            <td>Riddick</td>
        </tr>
        <tr>
            <td>Nolan Gerard Funk</td>
            <td>Riddick</td>
        </tr>
        <tr>
            <td>Andreas Apergis</td>
            <td>Riddick</td>
        </tr>
        <tr>
            <td>Neil Napier</td>
            <td>Riddick</td>
        </tr>
        <tr>
            <td>Danny Blanco Hall</td>
            <td>Riddick</td>
        </tr>
        <tr>
            <td>Matthew Nable</td>
            <td>Riddick</td>
        </tr>
        <tr>
            <td>Noah Danby</td>
            <td>Riddick</td>
        </tr>
        <tr>
            <td>Keri Hilson</td>
            <td>Riddick</td>
        </tr>
        <tr>
            <td>Dave Bautista</td>
            <td>Riddick</td>
        </tr>
        <tr>
            <td>Antoinette Kalaj</td>
            <td>Riddick</td>
        </tr>
        <tr>
            <td>Glenn Close</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Djimon Hounsou</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Tomas Arana</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Benicio del Toro</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Gregg Henry</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>John C. Reilly</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Stan Lee</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Christopher Fairbank</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Zoe Saldana</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Peter Serafinowicz</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Michael Rooker</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Seth Green</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>James Gunn</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Tyler Bates</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Rob Zombie</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Josh Brolin</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Spencer Wilding</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Richard Katz</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Bradley Cooper</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Sean Gunn</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Nathan Fillion</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Enoch Frost</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Laura Ortiz</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Lee Pace</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Brendan Fehr</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Chris Pratt</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Lloyd Kaufman</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Ophelia Lovibond</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Stephen Blackehart</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Enzo Cilenti</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Graham Shiels</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Tom Proctor</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Ralph Ineson</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Laura Haddock</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>John Brotherton</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Karen Gillan</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Dave Bautista</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Emmett Scanlan</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Ronan Summers</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Melia Kreiling</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Wyatt Oleff</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Nicole Alexandra Shipley</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Max Wrottesley</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Alexis Rodney</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Sharif Atkins</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Alexis Denisof</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Bruce Mackinnon</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Naomi Ryan</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Keeley Forsyth</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Nick Holmes</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Miriam Lucia</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Marama Corlett</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>David Yarovesky</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Mikaela Hoover</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Jozef Aoki</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Ekaterina Zalitko</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Krystian Godlewski</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Janis Ahern</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Solomon Mousley</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Lindsay Morton</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Robert Firth</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Dominic Grant</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Alison Lintott</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Frank Gilhooley</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Rosie Jones</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Abidemi Sobande</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Alex Rose</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Emily Redding</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Fred</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Jennifer Moylan-Taylor</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Douglas Robson</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Rachel Cullen</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Isabella Poynton</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Imogen Poynton</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Raed Abbas</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Freddie Andrews</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Helen Banks</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Erica Melargo</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Marianna Dean</td>
            <td>Guardians of the Galaxy</td>
        </tr>
        <tr>
            <td>Lucas Black</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Djimon Hounsou</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Jason Statham</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Kurt Russell</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Paul Walker</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Tyrese Gibson</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Ludacris</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Michelle Rodriguez</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Dwayne Johnson</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Jordana Brewster</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Noel Gugliemi</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Brian Mahoney</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Tony Jaa</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Shad Moss</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Sung Kang</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Elsa Pataky</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Jocelin Donahue</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Steve Coulter</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Gal Gadot</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Don Omar</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Tego Calder+¦n</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Robert Pralgo</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Luke Evans</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Nathalie Kelley</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Anna Colwell</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>John Brotherton</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>T-Pain</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Ali Fazal</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Ronda Rousey</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Benjamin Blankenship</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Brittney Alger</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Jon Lee Brody</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Nathalie Emmanuel</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Iggy Azalea</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Antwan Mills</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Sara Sohn</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Jorge-Luis Pallo</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Romeo Santos</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Cody Walker</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Eden Estrella</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Gentry White</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Klement Tinaj</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Levy Tran</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Caleb Walker</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Miller Kimsey</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Charlie Kimsey</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Viktor Hernandez</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>J.J. Phillips</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Jorge Ferragut</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>D.J. Hapa</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Stephanie Langston</td>
            <td>Furious 7</td>
        </tr>
        <tr>
            <td>Elijah Wood</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>Michael Caine</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>Isaach De Bankol+¬</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>Rena Owen</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>Kurt Angle</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>Dawn Olivieri</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>Stephanie Bertoni</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>+ôlafur Darri +ôlafsson</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>Inbar Lavi</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>Joseph Gilgun</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>Julie Engelbrecht</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>Jack Erdie</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>David Whalen</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>Rose Leslie</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>Aimee Carrero</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>Bex Taylor-Klaus</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>Allegra Carpenter</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>Armani Jackson</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>Samara Lee</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>Sloane Coombs</td>
            <td>The Last Witch Hunter</td>
        </tr>
        <tr>
            <td>Toussaint Raphael Abessolo</td>
            <td>The Last Witch Hunter</td>
        </tr>
    </tbody>
</table>



 List all the characters played by Vin Diesel and the movie they were in.


```sql
%%sql 
SELECT m.title,c.characters,STRFTIME('%Y',m.release_date) AS year
FROM movies AS m
JOIN casts AS c
    ON c.movie_id=m.movie_id
JOIN actors AS a
    ON a.actor_id=c.actor_id
WHERE a.actor_name='Vin Diesel' 
ORDER BY year LIMIT 15;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>characters</th>
            <th>year</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>The Fifth Element</td>
            <td>Finger (voice)</td>
            <td>1997</td>
        </tr>
        <tr>
            <td>Saving Private Ryan</td>
            <td>Private Adrian Caparzo</td>
            <td>1998</td>
        </tr>
        <tr>
            <td>The Iron Giant</td>
            <td>The Iron Giant (voice)</td>
            <td>1999</td>
        </tr>
        <tr>
            <td>Pitch Black</td>
            <td>Richard B. Riddick</td>
            <td>2000</td>
        </tr>
        <tr>
            <td>Boiler Room</td>
            <td>Chris Varick</td>
            <td>2000</td>
        </tr>
        <tr>
            <td>Knockaround Guys</td>
            <td>Taylor Reese</td>
            <td>2001</td>
        </tr>
        <tr>
            <td>The Fast and the Furious</td>
            <td>Dominic Toretto</td>
            <td>2001</td>
        </tr>
        <tr>
            <td>xXx</td>
            <td>Xander Cage</td>
            <td>2002</td>
        </tr>
        <tr>
            <td>A Man Apart</td>
            <td>Sean Vetter</td>
            <td>2003</td>
        </tr>
        <tr>
            <td>The Chronicles of Riddick</td>
            <td>Riddick</td>
            <td>2004</td>
        </tr>
        <tr>
            <td>The Pacifier</td>
            <td>Shane Wolf</td>
            <td>2005</td>
        </tr>
        <tr>
            <td>The Fast and the Furious: Tokyo Drift</td>
            <td>Dominic Toretto (uncredited)</td>
            <td>2006</td>
        </tr>
        <tr>
            <td>Find Me Guilty</td>
            <td>Jackie DiNorscio</td>
            <td>2006</td>
        </tr>
        <tr>
            <td>Babylon A.D.</td>
            <td>Toorop</td>
            <td>2008</td>
        </tr>
        <tr>
            <td>Fast Five</td>
            <td>Dominic Toretto</td>
            <td>2011</td>
        </tr>
    </tbody>
</table>



What are the titles of all the movies Vin Diesel has been in?


```sql
%%sql
SELECT m.title, a.actor_name,STRFTIME('%Y',m.release_date) AS year
FROM movies AS m
JOIN casts AS c
    ON c.movie_id=m.movie_id
JOIN actors AS a
    ON a.actor_id=c.actor_id
WHERE a.actor_name='Vin Diesel' 
ORDER BY year LIMIT 15;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>actor_name</th>
            <th>year</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>The Fifth Element</td>
            <td>Vin Diesel</td>
            <td>1997</td>
        </tr>
        <tr>
            <td>Saving Private Ryan</td>
            <td>Vin Diesel</td>
            <td>1998</td>
        </tr>
        <tr>
            <td>The Iron Giant</td>
            <td>Vin Diesel</td>
            <td>1999</td>
        </tr>
        <tr>
            <td>Pitch Black</td>
            <td>Vin Diesel</td>
            <td>2000</td>
        </tr>
        <tr>
            <td>Boiler Room</td>
            <td>Vin Diesel</td>
            <td>2000</td>
        </tr>
        <tr>
            <td>Knockaround Guys</td>
            <td>Vin Diesel</td>
            <td>2001</td>
        </tr>
        <tr>
            <td>The Fast and the Furious</td>
            <td>Vin Diesel</td>
            <td>2001</td>
        </tr>
        <tr>
            <td>xXx</td>
            <td>Vin Diesel</td>
            <td>2002</td>
        </tr>
        <tr>
            <td>A Man Apart</td>
            <td>Vin Diesel</td>
            <td>2003</td>
        </tr>
        <tr>
            <td>The Chronicles of Riddick</td>
            <td>Vin Diesel</td>
            <td>2004</td>
        </tr>
        <tr>
            <td>The Pacifier</td>
            <td>Vin Diesel</td>
            <td>2005</td>
        </tr>
        <tr>
            <td>The Fast and the Furious: Tokyo Drift</td>
            <td>Vin Diesel</td>
            <td>2006</td>
        </tr>
        <tr>
            <td>Find Me Guilty</td>
            <td>Vin Diesel</td>
            <td>2006</td>
        </tr>
        <tr>
            <td>Babylon A.D.</td>
            <td>Vin Diesel</td>
            <td>2008</td>
        </tr>
        <tr>
            <td>Fast Five</td>
            <td>Vin Diesel</td>
            <td>2011</td>
        </tr>
    </tbody>
</table>



Find the actors in the movie Deadpool AND YEAR OF RELEASE WE USE STRFTIME('%Y',Column_name) we just need the year only


```sql
%%sql
SELECT a.actor_name,m.title,STRFTIME('%Y', m.release_date) AS release_year
FROM movies AS m
JOIN casts AS c
ON c.movie_id=m.movie_id
JOIN actors AS a
ON a.actor_id=c.actor_id
WHERE m.title='Deadpool'
ORDER BY a.actor_name DESC LIMIT 10;

```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>actor_name</th>
            <th>title</th>
            <th>release_year</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Victoria De Mare</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Tony Chris Kazoleas</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Taylor Hickson</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>T.J. Miller</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Style Dayne</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Stefan Kapi-ìi-ç</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Stan Lee</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Sean Quan</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Ryan Reynolds</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Rob Hayter</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
    </tbody>
</table>



Show the role played by the actor Ryan Reynolds AND Stan Lee


```sql
%%sql
SELECT DISTINCT(o.award),a.actor_name,m.title,STRFTIME('%Y',m.release_date) AS release_year
FROM oscars AS o
JOIN movies AS m
   ON m.movie_id=c.movie_id
JOIN casts AS c
    ON c.movie_id=m.movie_id
JOIN actors AS a
    ON a.actor_id=c.actor_id
WHERE a.actor_name IN ('Ryan Reynolds' , 'Stan Lee')
    AND m.title='Deadpool'LIMIT 10;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>award</th>
            <th>actor_name</th>
            <th>title</th>
            <th>release_year</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Actor</td>
            <td>Stan Lee</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Actor</td>
            <td>Ryan Reynolds</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Actress</td>
            <td>Stan Lee</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Actress</td>
            <td>Ryan Reynolds</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Art Direction</td>
            <td>Stan Lee</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Art Direction</td>
            <td>Ryan Reynolds</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Cinematography</td>
            <td>Stan Lee</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Cinematography</td>
            <td>Ryan Reynolds</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Directing (Comedy Picture)</td>
            <td>Stan Lee</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Directing (Comedy Picture)</td>
            <td>Ryan Reynolds</td>
            <td>Deadpool</td>
            <td>2016</td>
        </tr>
    </tbody>
</table>



Find the actors in the movie Deadpool AND YEAR OF RELEASE and the role played by each actor


```sql
%%sql
SELECT a.actor_name,m.title,o.award,STRFTIME('%Y',m.release_date)
FROM oscars AS o
JOIN movies AS m 
    ON m.movie_id=c.movie_id
JOIN casts AS c 
    ON c.movie_id=m.movie_id
JOIN actors AS a
    ON a.actor_id=c.actor_id
WHERE m.title='Deadpool'
    AND o.award IN ('Actor','Actress') 
ORDER by a.actor_name DESC LIMIT 10;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>actor_name</th>
            <th>title</th>
            <th>award</th>
            <th>STRFTIME(&#x27;%Y&#x27;,m.release_date)</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Victoria De Mare</td>
            <td>Deadpool</td>
            <td>Actor</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Victoria De Mare</td>
            <td>Deadpool</td>
            <td>Actor</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Victoria De Mare</td>
            <td>Deadpool</td>
            <td>Actress</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Victoria De Mare</td>
            <td>Deadpool</td>
            <td>Actress</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Victoria De Mare</td>
            <td>Deadpool</td>
            <td>Actress</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Victoria De Mare</td>
            <td>Deadpool</td>
            <td>Actor</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Victoria De Mare</td>
            <td>Deadpool</td>
            <td>Actor</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Victoria De Mare</td>
            <td>Deadpool</td>
            <td>Actor</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Victoria De Mare</td>
            <td>Deadpool</td>
            <td>Actor</td>
            <td>2016</td>
        </tr>
        <tr>
            <td>Victoria De Mare</td>
            <td>Deadpool</td>
            <td>Actor</td>
            <td>2016</td>
        </tr>
    </tbody>
</table>



Show the key_id where the keyword is love.


```sql
%%sql
SELECT keyword_id,keyword_name
FROM keywords 
WHERE keyword_name LIKE '%love%';
```

Find the movie with it is genre name with the keyword love


```sql
%%sql
---we find the movie title,Genre. meaning we connect four tables
SELECT m.title,g.genre_name,kw.keyword_name,STRFTIME('%Y', m.release_date) AS release_year
FROM movies AS m
JOIN genremap AS gm
ON gm.movie_id=m.movie_id
JOIN genres AS g
ON g.genre_id=gm.genre_id
JOIN keywordmap AS kwm
ON kwm.movie_id=m.movie_id
JOIN keywords AS kw
ON kw.keyword_id=kwm.keyword_id
WHERE kw.keyword_name LIKE '%love%'
LIMIT 20;

```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>genre_name</th>
            <th>keyword_name</th>
            <th>release_year</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Match Point</td>
            <td>Drama</td>
            <td>love triangle</td>
            <td>2005</td>
        </tr>
        <tr>
            <td>Match Point</td>
            <td>Thriller</td>
            <td>love triangle</td>
            <td>2005</td>
        </tr>
        <tr>
            <td>Match Point</td>
            <td>Crime</td>
            <td>love triangle</td>
            <td>2005</td>
        </tr>
        <tr>
            <td>Match Point</td>
            <td>Romance</td>
            <td>love triangle</td>
            <td>2005</td>
        </tr>
        <tr>
            <td>AmTlie</td>
            <td>Comedy</td>
            <td>love triangle</td>
            <td>2001</td>
        </tr>
        <tr>
            <td>AmTlie</td>
            <td>Romance</td>
            <td>love triangle</td>
            <td>2001</td>
        </tr>
        <tr>
            <td>Casablanca</td>
            <td>Drama</td>
            <td>love triangle</td>
            <td>1942</td>
        </tr>
        <tr>
            <td>Casablanca</td>
            <td>Romance</td>
            <td>love triangle</td>
            <td>1942</td>
        </tr>
        <tr>
            <td>The Horse Whisperer</td>
            <td>Drama</td>
            <td>love triangle</td>
            <td>1998</td>
        </tr>
        <tr>
            <td>The Horse Whisperer</td>
            <td>Romance</td>
            <td>love triangle</td>
            <td>1998</td>
        </tr>
        <tr>
            <td>The Piano</td>
            <td>Drama</td>
            <td>love triangle</td>
            <td>1993</td>
        </tr>
        <tr>
            <td>The Piano</td>
            <td>Romance</td>
            <td>love triangle</td>
            <td>1993</td>
        </tr>
        <tr>
            <td>Doctor Zhivago</td>
            <td>Drama</td>
            <td>love triangle</td>
            <td>1965</td>
        </tr>
        <tr>
            <td>Doctor Zhivago</td>
            <td>Romance</td>
            <td>love triangle</td>
            <td>1965</td>
        </tr>
        <tr>
            <td>Doctor Zhivago</td>
            <td>War</td>
            <td>love triangle</td>
            <td>1965</td>
        </tr>
        <tr>
            <td>The Rules of Attraction</td>
            <td>Drama</td>
            <td>love triangle</td>
            <td>2002</td>
        </tr>
        <tr>
            <td>The Rules of Attraction</td>
            <td>Comedy</td>
            <td>love triangle</td>
            <td>2002</td>
        </tr>
        <tr>
            <td>The Rules of Attraction</td>
            <td>Romance</td>
            <td>love triangle</td>
            <td>2002</td>
        </tr>
        <tr>
            <td>Keeping the Faith</td>
            <td>Comedy</td>
            <td>love triangle</td>
            <td>2000</td>
        </tr>
        <tr>
            <td>Days of Heaven</td>
            <td>Drama</td>
            <td>love triangle</td>
            <td>1978</td>
        </tr>
    </tbody>
</table>



Show the production country, production company for movies produced  with the highest rating and there year they where released and status.


```sql
%%sql
SELECT
    pco.production_country_name,
    pc.production_company_name,
    m.title,
    pco.iso_3166_1,
    m.release_status,
    m.popularity,
    STRFTIME('%Y', m.release_date) AS release_year
FROM movies AS m
JOIN productioncountrymap AS pcom
    ON pcom.movie_id = m.movie_id
JOIN productioncountries AS pco
    ON pco.iso_3166_1 = pcom.iso_3166_1
JOIN productioncompanymap AS pcm
    ON pcm.movie_id = m.movie_id
JOIN productioncompanies AS pc
    ON pc.production_company_id = pcm.production_company_id
WHERE m.popularity = (
    SELECT MAX(popularity) 
    FROM movies
)
ORDER BY m.popularity DESC;

```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>production_country_name</th>
            <th>production_company_name</th>
            <th>title</th>
            <th>iso_3166_1</th>
            <th>release_status</th>
            <th>popularity</th>
            <th>release_year</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>United States of America</td>
            <td>Universal Pictures</td>
            <td>Minions</td>
            <td>US</td>
            <td>Released</td>
            <td>875.581305</td>
            <td>2015</td>
        </tr>
        <tr>
            <td>United States of America</td>
            <td>Illumination Entertainment</td>
            <td>Minions</td>
            <td>US</td>
            <td>Released</td>
            <td>875.581305</td>
            <td>2015</td>
        </tr>
    </tbody>
</table>



List movies that have no ratings.


```sql
%%sql
SELECT title
FROM movies
WHERE popularity='0.0' OR popularity = '';
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>America Is Still the Place</td>
        </tr>
    </tbody>
</table>



Find the production Company who has directed the most movies:


```sql
%%sql
SELECT pc.production_company_name,COUNT(m.movie_id) AS movies_directed
FROM movies AS m
JOIN productioncompanymap AS pcm 
ON pcm.movie_id=m.movie_id
JOIN productioncompanies AS pc
ON pc.production_company_id=pcm.production_company_id
GROUP BY pc.production_company_name
ORDER BY movies_directed DESC LIMIT 5;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>production_company_name</th>
            <th>movies_directed</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Warner Bros.</td>
            <td>319</td>
        </tr>
        <tr>
            <td>Universal Pictures</td>
            <td>311</td>
        </tr>
        <tr>
            <td>Paramount Pictures</td>
            <td>285</td>
        </tr>
        <tr>
            <td>Twentieth Century Fox Film Corporation</td>
            <td>222</td>
        </tr>
        <tr>
            <td>Columbia Pictures</td>
            <td>201</td>
        </tr>
    </tbody>
</table>



List movie titles and their average star rating, ordered by highest rating:


```sql
%%sql
SELECT title,ROUND(AVG(popularity),0) AS Average_star_rating
FROM movies 
GROUP BY title
ORDER BY Average_star_rating DESC 
LIMIT 10;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>Average_star_rating</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Minions</td>
            <td>876.0</td>
        </tr>
        <tr>
            <td>Interstellar</td>
            <td>724.0</td>
        </tr>
        <tr>
            <td>Deadpool</td>
            <td>515.0</td>
        </tr>
        <tr>
            <td>Guardians of the Galaxy</td>
            <td>481.0</td>
        </tr>
        <tr>
            <td>Mad Max: Fury Road</td>
            <td>434.0</td>
        </tr>
        <tr>
            <td>Jurassic World</td>
            <td>419.0</td>
        </tr>
        <tr>
            <td>Pirates of the Caribbean: The Curse of the Black Pearl</td>
            <td>272.0</td>
        </tr>
        <tr>
            <td>Dawn of the Planet of the Apes</td>
            <td>244.0</td>
        </tr>
        <tr>
            <td>The Hunger Games: Mockingjay - Part 1</td>
            <td>206.0</td>
        </tr>
        <tr>
            <td>Big Hero 6</td>
            <td>204.0</td>
        </tr>
    </tbody>
</table>



Find movie names where actors acted in two or more movies: 



```sql
%%sql
SELECT m.title,a.actor_id
FROM movies AS m
JOIN casts AS c
ON m.movie_id=c.movie_id
JOIN actors AS a
ON a.actor_id=c.actor_id
GROUP BY m.title
HAVING COUNT(DISTINCT a.actor_id)>=2
LIMIT 15;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>actor_id</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>#Horror</td>
            <td>343</td>
        </tr>
        <tr>
            <td>(500) Days of Summer</td>
            <td>5375</td>
        </tr>
        <tr>
            <td>10 Cloverfield Lane</td>
            <td>1230</td>
        </tr>
        <tr>
            <td>10 Days in a Madhouse</td>
            <td>38559</td>
        </tr>
        <tr>
            <td>10 Things I Hate About You</td>
            <td>19</td>
        </tr>
        <tr>
            <td>102 Dalmatians</td>
            <td>515</td>
        </tr>
        <tr>
            <td>10th &amp; Wolf</td>
            <td>1771</td>
        </tr>
        <tr>
            <td>11:14</td>
            <td>448</td>
        </tr>
        <tr>
            <td>12 Angry Men</td>
            <td>1936</td>
        </tr>
        <tr>
            <td>12 Rounds</td>
            <td>2202</td>
        </tr>
        <tr>
            <td>12 Years a Slave</td>
            <td>287</td>
        </tr>
        <tr>
            <td>127 Hours</td>
            <td>639</td>
        </tr>
        <tr>
            <td>13 Going on 30</td>
            <td>103</td>
        </tr>
        <tr>
            <td>13 Hours: The Secret Soldiers of Benghazi</td>
            <td>10881</td>
        </tr>
        <tr>
            <td>1408</td>
            <td>1980</td>
        </tr>
    </tbody>
</table>



List actors who acted in movies before 2000 and after 2015: 



```sql
%%sql
SELECT DISTINCT a.actor_name,m.title,o.year
FROM oscars AS o
JOIN movies AS m ON m.movie_id=c.movie_id
JOIN casts AS c ON c.movie_id=m.movie_id
JOIN actors AS a ON a.actor_id=c.actor_id
WHERE o.year IN('2000','2015') 
LIMIT 20;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>actor_name</th>
            <th>title</th>
            <th>year</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Bruce Willis</td>
            <td>Four Rooms</td>
            <td>2000</td>
        </tr>
        <tr>
            <td>Bruce Willis</td>
            <td>Four Rooms</td>
            <td>2015</td>
        </tr>
        <tr>
            <td>Quentin Tarantino</td>
            <td>Four Rooms</td>
            <td>2000</td>
        </tr>
        <tr>
            <td>Quentin Tarantino</td>
            <td>Four Rooms</td>
            <td>2015</td>
        </tr>
        <tr>
            <td>Lawrence Bender</td>
            <td>Four Rooms</td>
            <td>2000</td>
        </tr>
        <tr>
            <td>Lawrence Bender</td>
            <td>Four Rooms</td>
            <td>2015</td>
        </tr>
        <tr>
            <td>David Proval</td>
            <td>Four Rooms</td>
            <td>2000</td>
        </tr>
        <tr>
            <td>David Proval</td>
            <td>Four Rooms</td>
            <td>2015</td>
        </tr>
        <tr>
            <td>Sammi Davis</td>
            <td>Four Rooms</td>
            <td>2000</td>
        </tr>
        <tr>
            <td>Sammi Davis</td>
            <td>Four Rooms</td>
            <td>2015</td>
        </tr>
        <tr>
            <td>Amanda de Cadenet</td>
            <td>Four Rooms</td>
            <td>2000</td>
        </tr>
        <tr>
            <td>Amanda de Cadenet</td>
            <td>Four Rooms</td>
            <td>2015</td>
        </tr>
        <tr>
            <td>Valeria Golino</td>
            <td>Four Rooms</td>
            <td>2000</td>
        </tr>
        <tr>
            <td>Valeria Golino</td>
            <td>Four Rooms</td>
            <td>2015</td>
        </tr>
        <tr>
            <td>Madonna</td>
            <td>Four Rooms</td>
            <td>2000</td>
        </tr>
        <tr>
            <td>Madonna</td>
            <td>Four Rooms</td>
            <td>2015</td>
        </tr>
        <tr>
            <td>Ione Skye</td>
            <td>Four Rooms</td>
            <td>2000</td>
        </tr>
        <tr>
            <td>Ione Skye</td>
            <td>Four Rooms</td>
            <td>2015</td>
        </tr>
        <tr>
            <td>Lili Taylor</td>
            <td>Four Rooms</td>
            <td>2000</td>
        </tr>
        <tr>
            <td>Lili Taylor</td>
            <td>Four Rooms</td>
            <td>2015</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT a.actor_name,m.title
FROM oscars AS o
JOIN movies AS m
ON m.movie_id=c.movie_id
JOIN casts AS c
ON c.movie_id=m.movie_id
JOIN actors AS a
ON a.actor_id=c.actor_id
WHERE o.year='2000'
INTERSECT
SELECT a.actor_name,m.title
FROM oscars AS o
JOIN movies AS m
ON m.movie_id=c.movie_id
JOIN casts AS c
ON c.movie_id=m.movie_id
JOIN actors AS a
ON a.actor_id=c.actor_id
WHERE o.year='2015' LIMIT 20;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>actor_name</th>
            <th>title</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>&nbsp;&nbsp;Jorge de los Reyes </td>
            <td>Guten Tag, Ram=n</td>
        </tr>
        <tr>
            <td> Larry Mullen Jr.</td>
            <td>U2 3D</td>
        </tr>
        <tr>
            <td>&quot;&quot;&quot;Weird Al&quot;&quot; Yankovic&quot;</td>
            <td>Halloween II</td>
        </tr>
        <tr>
            <td>&quot;&quot;&quot;Weird Al&quot;&quot; Yankovic&quot;</td>
            <td>Spy Hard</td>
        </tr>
        <tr>
            <td>&quot;&quot;&quot;Weird Al&quot;&quot; Yankovic&quot;</td>
            <td>The Naked Gun 33?: The Final Insult</td>
        </tr>
        <tr>
            <td>&quot;&quot;&quot;Weird Al&quot;&quot; Yankovic&quot;</td>
            <td>UHF</td>
        </tr>
        <tr>
            <td>&quot;Brendan &quot;&quot;Doogie&quot;&quot; Milewski&quot;</td>
            <td>Burn</td>
        </tr>
        <tr>
            <td>&quot;Chad &quot;&quot;Gunner&quot;&quot; Lail&quot;</td>
            <td>Navy Seals vs. Zombies</td>
        </tr>
        <tr>
            <td>&quot;Dennis &quot;&quot;Bumpy&quot;&quot; Kanahele&quot;</td>
            <td>Aloha</td>
        </tr>
        <tr>
            <td>&quot;Donald &quot;&quot;Duck&quot;&quot; Dunn&quot;</td>
            <td>The Blues Brothers</td>
        </tr>
        <tr>
            <td>&quot;Jason &quot;&quot;Jake&quot;&quot; Cross&quot;</td>
            <td>Boogie Nights</td>
        </tr>
        <tr>
            <td>&quot;Larry &quot;&quot;Flash&quot;&quot; Jenkins&quot;</td>
            <td>Edtv</td>
        </tr>
        <tr>
            <td>&quot;Larry &quot;&quot;Flash&quot;&quot; Jenkins&quot;</td>
            <td>Prison</td>
        </tr>
        <tr>
            <td>&quot;Noah &quot;&quot;40&quot;&quot; Shebib&quot;</td>
            <td>The Virgin Suicides</td>
        </tr>
        <tr>
            <td>&quot;Rodney &quot;&quot;Bear&quot;&quot; Jackson&quot;</td>
            <td>Inside Man</td>
        </tr>
        <tr>
            <td>&quot;Rodney &quot;&quot;Bear&quot;&quot; Jackson&quot;</td>
            <td>The Hurricane</td>
        </tr>
        <tr>
            <td>&quot;Rozonda &quot;&quot;Chilli&quot;&quot; Thomas&quot;</td>
            <td>Snow Day</td>
        </tr>
        <tr>
            <td>&quot;Timothy &quot;&quot;Speed&quot;&quot; Levitch&quot;</td>
            <td>School of Rock</td>
        </tr>
        <tr>
            <td>&#x27;Snub&#x27; Pollard</td>
            <td>Pocketful of Miracles</td>
        </tr>
        <tr>
            <td>&#x27;Snub&#x27; Pollard</td>
            <td>Singin&#x27; in the Rain</td>
        </tr>
    </tbody>
</table>



List all movies released before a certain year (e.g., 1998):


```sql
%%sql
SELECT title,release_date
FROM movies
WHERE release_date BETWEEN '1998-01-01' AND '1998-12-31' 
LIMIT 10;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>release_date</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>American History X</td>
            <td>1998-10-30 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Armageddon</td>
            <td>1998-07-01 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Lock, Stock and Two Smoking Barrels</td>
            <td>1998-03-05 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Run Lola Run</td>
            <td>1998-08-20 00:00:00.000000</td>
        </tr>
        <tr>
            <td>The Big Lebowski</td>
            <td>1998-03-06 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Star Trek: Insurrection</td>
            <td>1998-12-10 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Meet Joe Black</td>
            <td>1998-11-12 00:00:00.000000</td>
        </tr>
        <tr>
            <td>The Celebration</td>
            <td>1998-05-20 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Pi</td>
            <td>1998-07-10 00:00:00.000000</td>
        </tr>
        <tr>
            <td>There&#x27;s Something About Mary</td>
            <td>1998-07-15 00:00:00.000000</td>
        </tr>
    </tbody>
</table>



Find the names of actors who acted in a specific movie (e.g., 'Annie Hall')


```sql
%%sql
SELECT a.actor_name,m.title,STRFTIME('%Y',m.release_date) AS year
FROM movies AS m
JOIN casts AS c
ON m.movie_id=c.movie_id
JOIN actors AS a
ON a.actor_id=c.actor_id
WHERE m.title='Annie Hall'
AND STRFTIME('%Y',m.release_date)='1977'
LIMIT 10;

```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>actor_name</th>
            <th>title</th>
            <th>year</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Paula Trueman</td>
            <td>Annie Hall</td>
            <td>1977</td>
        </tr>
        <tr>
            <td>Beverly D&#x27;Angelo</td>
            <td>Annie Hall</td>
            <td>1977</td>
        </tr>
        <tr>
            <td>Woody Allen</td>
            <td>Annie Hall</td>
            <td>1977</td>
        </tr>
        <tr>
            <td>Mark Lenard</td>
            <td>Annie Hall</td>
            <td>1977</td>
        </tr>
        <tr>
            <td>Truman Capote</td>
            <td>Annie Hall</td>
            <td>1977</td>
        </tr>
        <tr>
            <td>Diane Keaton</td>
            <td>Annie Hall</td>
            <td>1977</td>
        </tr>
        <tr>
            <td>Shelley Hack</td>
            <td>Annie Hall</td>
            <td>1977</td>
        </tr>
        <tr>
            <td>Tracey Walter</td>
            <td>Annie Hall</td>
            <td>1977</td>
        </tr>
        <tr>
            <td>Christopher Walken</td>
            <td>Annie Hall</td>
            <td>1977</td>
        </tr>
        <tr>
            <td>Jeff Goldblum</td>
            <td>Annie Hall</td>
            <td>1977</td>
        </tr>
    </tbody>
</table>



1.Find Actor_id,first name is 'Woody' and last name is 'Allen'

List all movies released before 2015.


```sql
%%sql
SELECT 
title,release_date,release_status
FROM movies
WHERE release_date>=2015 LIMIT 10;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>release_date</th>
            <th>release_status</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Four Rooms</td>
            <td>1995-12-09 00:00:00.000000</td>
            <td>Released</td>
        </tr>
        <tr>
            <td>Star Wars</td>
            <td>1977-05-25 00:00:00.000000</td>
            <td>Released</td>
        </tr>
        <tr>
            <td>Finding Nemo</td>
            <td>2003-05-30 00:00:00.000000</td>
            <td>Released</td>
        </tr>
        <tr>
            <td>Forrest Gump</td>
            <td>1994-07-06 00:00:00.000000</td>
            <td>Released</td>
        </tr>
        <tr>
            <td>American Beauty</td>
            <td>1999-09-15 00:00:00.000000</td>
            <td>Released</td>
        </tr>
        <tr>
            <td>Dancer in the Dark</td>
            <td>2000-05-17 00:00:00.000000</td>
            <td>Released</td>
        </tr>
        <tr>
            <td>The Fifth Element</td>
            <td>1997-05-07 00:00:00.000000</td>
            <td>Released</td>
        </tr>
        <tr>
            <td>Metropolis</td>
            <td>1927-01-10 00:00:00.000000</td>
            <td>Released</td>
        </tr>
        <tr>
            <td>My Life Without Me</td>
            <td>2003-03-07 00:00:00.000000</td>
            <td>Released</td>
        </tr>
        <tr>
            <td>Pirates of the Caribbean: The Curse of the Black Pearl</td>
            <td>2003-07-09 00:00:00.000000</td>
            <td>Released</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
PRAGMA table_info(actors);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>actor_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>actor_name</td>
            <td>varchar(100)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2</td>
            <td>gender</td>
            <td>INTEGER</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT actor_id,actor_name,gender
FROM actors
WHERE actor_name LIKE '%Woody%%Allen%';

```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>actor_id</th>
            <th>actor_name</th>
            <th>gender</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1243</td>
            <td>Woody Allen</td>
            <td>2</td>
        </tr>
    </tbody>
</table>



2.Find movie_id,title,year with the word Boogie Nights


```sql
%%sql
SELECT o.year,m.movie_id,m.title
FROM oscars AS o
JOIN movies AS m 
ON c.movie_id=m.movie_id
JOIN  casts AS c
ON c.movie_id=m.movie_id
WHERE title LIKE '%Boogie%%Nights%' LIMIT 5;

```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>year</th>
            <th>movie_id</th>
            <th>title</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1928</td>
            <td>4995</td>
            <td>Boogie Nights</td>
        </tr>
        <tr>
            <td>1928</td>
            <td>4995</td>
            <td>Boogie Nights</td>
        </tr>
        <tr>
            <td>1928</td>
            <td>4995</td>
            <td>Boogie Nights</td>
        </tr>
        <tr>
            <td>1928</td>
            <td>4995</td>
            <td>Boogie Nights</td>
        </tr>
        <tr>
            <td>1928</td>
            <td>4995</td>
            <td>Boogie Nights</td>
        </tr>
    </tbody>
</table>



Find actors who never acted in a “Science Fiction” movie.


```sql
%%sql
SELECT a.actor_name,g.genre_name
FROM movies AS m
JOIN genremap AS gm
ON gm.movie_id=m.movie_id
JOIN genres AS g
ON g.genre_id=gm.genre_id
JOIN casts AS c
ON c.movie_id=m.movie_id
JOIN actors AS a
ON a.actor_id=c.actor_id
WHERE g.genre_name='Science Fiction' LIMIT 10;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>actor_name</th>
            <th>genre_name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Mark Hamill</td>
            <td>Science Fiction</td>
        </tr>
        <tr>
            <td>Harrison Ford</td>
            <td>Science Fiction</td>
        </tr>
        <tr>
            <td>Carrie Fisher</td>
            <td>Science Fiction</td>
        </tr>
        <tr>
            <td>Peter Cushing</td>
            <td>Science Fiction</td>
        </tr>
        <tr>
            <td>Anthony Daniels</td>
            <td>Science Fiction</td>
        </tr>
        <tr>
            <td>Kenny Baker</td>
            <td>Science Fiction</td>
        </tr>
        <tr>
            <td>William Hootkins</td>
            <td>Science Fiction</td>
        </tr>
        <tr>
            <td>Scott Beach</td>
            <td>Science Fiction</td>
        </tr>
        <tr>
            <td>Joe Johnston</td>
            <td>Science Fiction</td>
        </tr>
        <tr>
            <td>Phil Tippett</td>
            <td>Science Fiction</td>
        </tr>
    </tbody>
</table>



Retrieve movies that have “Anne Hathaway” and “Matthew McConaughey.”


```sql
%%sql
SELECT m.title,a.actor_name
FROM movies AS m
JOIN casts AS c 
ON c.movie_id=m.movie_id
JOIN actors AS a
ON a.actor_id=c.actor_id
WHERE a.actor_name LIKE'%Anne Hathaway%'
OR a.actor_name LIKE '%Matthew McConaughey%';

```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>actor_name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Brokeback Mountain</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>The Devil Wears Prada</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>Contact</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>A Time to Kill</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>The Wedding Planner</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Becoming Jane</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>U-571</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Reign of Fire</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Failure to Launch</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Sahara</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Tropic Thunder</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Fool&#x27;s Gold</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Dazed and Confused</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>The Princess Diaries</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>How to Lose a Guy in 10 Days</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Bride Wars</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>Hoodwinked!</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>The Princess Diaries 2: Royal Engagement</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>We Are Marshall</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Edtv</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Get Smart</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>Amistad</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Frailty</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Alice in Wonderland</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>Surfer, Dude</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Ella Enchanted</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>Rachel Getting Married</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>Paparazzi</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>The Other Side of Heaven</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>Thirteen Conversations About One Thing</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Lone Star</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Nicholas Nickleby</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>Valentine&#x27;s Day</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>The Newton Boys</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Love &amp; Other Drugs</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>Rio</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>The Dark Knight Rises</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>The Lincoln Lawyer</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>One Day</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>Killer Joe</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Magic Mike</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Les MisTrables</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>Bernie</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Mud</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>The Wolf of Wall Street</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Don Jon</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>Dallas Buyers Club</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Interstellar</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>Interstellar</td>
            <td>Matthew McConaughey</td>
        </tr>
        <tr>
            <td>Rio 2</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>Alice Through the Looking Glass</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>Song One</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>The Intern</td>
            <td>Anne Hathaway</td>
        </tr>
        <tr>
            <td>Free State of Jones</td>
            <td>Matthew McConaughey</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT title,movie_id,overview --- this are the columns we need.
FROM movies
WHERE movie_id IN('905','927','947');---Here we apply IN, this combines them into a single query.
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>movie_id</th>
            <th>overview</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Pandora&#x27;s Box</td>
            <td>905</td>
            <td>The rise and inevitable fall of an amoral but naive young woman whose insouciant eroticism inspires lust and violence in those around her.</td>
        </tr>
        <tr>
            <td>Gremlins</td>
            <td>927</td>
            <td>When Billy Peltzer is given a strange but adorable pet named Gizmo for Christmas, he inadvertently breaks the three important rules of caring for a Mogwai, and unleashes a horde of mischievous gremlins on a small town.</td>
        </tr>
        <tr>
            <td>Lawrence of Arabia</td>
            <td>947</td>
            <td>An epic about British officer T.E. Lawrence&#x27;s mission to aid the Arab tribes in their revolt against the Ottoman Empire during the First World War. Lawrence becomes a flamboyant, messianic figure in the cause of Arab unity but his psychological instability threatens to undermine his achievements.</td>
        </tr>
    </tbody>
</table>



Alternative method.


```sql
%%sql
SELECT title,movie_id
FROM movies
WHERE movie_id='905'
OR movie_id='907'---we use OR to give an option 
OR movie_id='947';
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>movie_id</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Pandora&#x27;s Box</td>
            <td>905</td>
        </tr>
        <tr>
            <td>Doctor Zhivago</td>
            <td>907</td>
        </tr>
        <tr>
            <td>Lawrence of Arabia</td>
            <td>947</td>
        </tr>
    </tbody>
</table>



4.Get the highest-rated movie in each genre.

WE look at the primary Key that joins the tables and sort in desc order


```sql
%%sql
SELECT m.movie_id,m.title,genre_name,
AVG(m.popularity) AS AVG_popularity
FROM movies AS m
JOIN genremap AS gm
ON m.movie_id=gm.movie_id     ---what connects the genres table to movies is the genremap table 
JOIN genres AS g
ON g.genre_id=gm.genre_id
GROUP BY m.movie_id
ORDER BY AVG_popularity DESC
LIMIT 5;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>movie_id</th>
            <th>title</th>
            <th>genre_name</th>
            <th>AVG_popularity</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>211672</td>
            <td>Minions</td>
            <td>Adventure</td>
            <td>875.581305</td>
        </tr>
        <tr>
            <td>157336</td>
            <td>Interstellar</td>
            <td>Adventure</td>
            <td>724.247784</td>
        </tr>
        <tr>
            <td>293660</td>
            <td>Deadpool</td>
            <td>Adventure</td>
            <td>514.569956</td>
        </tr>
        <tr>
            <td>118340</td>
            <td>Guardians of the Galaxy</td>
            <td>Adventure</td>
            <td>481.098624</td>
        </tr>
        <tr>
            <td>76341</td>
            <td>Mad Max: Fury Road</td>
            <td>Adventure</td>
            <td>434.278564</td>
        </tr>
    </tbody>
</table>




```sql
%%sql 
PRAGMA table_info(genres);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>genre_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>genre_name</td>
            <td>varchar(50)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>



5.find Movies with the following Ids 955,907,9515


```sql
%%sql
SELECT title,movie_id,overview 
FROM movies
WHERE movie_id IN ('955','907','9515')
ORDER BY movie_id DESC;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>movie_id</th>
            <th>overview</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>The Matador</td>
            <td>9515</td>
            <td>The life of Danny Wright, a salesman forever on the road, veers into dangerous and surreal territory when he wanders into a Mexican bar and meets a mysterious stranger, Julian, who&#x27;s very likely a hit man. Their meeting sets off a chain of events that will change their lives forever, as Wright is suddenly thrust into a far-from-mundane existence that he takes to surprisingly well à once he gets acclimated to it.</td>
        </tr>
        <tr>
            <td>Mission: Impossible II</td>
            <td>955</td>
            <td>With computer genius Luther Stickell at his side and a beautiful thief on his mind, agent Ethan Hunt races across Australia and Spain to stop a former IMF agent from unleashing a genetically engineered biological weapon called Chimera. This mission, should Hunt choose to accept it, plunges him into the center of an international crisis of terrifying magnitude.</td>
        </tr>
        <tr>
            <td>Doctor Zhivago</td>
            <td>907</td>
            <td>Doctor Zhivago is the filmed adapation of the Russian novel by Boris Pasternak from director David Lean that was an international success and today deemed a classic. Omar Sharif and Julie Christie play two protagonists who in fact love each other yet because of their current situation cannot find a way be together.</td>
        </tr>
    </tbody>
</table>



6.Display all movies in the ‘Drama’ genre.


```sql
%%sql
SELECT* FROM genres;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>genre_id</th>
            <th>genre_name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>12</td>
            <td>Adventure</td>
        </tr>
        <tr>
            <td>14</td>
            <td>Fantasy</td>
        </tr>
        <tr>
            <td>16</td>
            <td>Animation</td>
        </tr>
        <tr>
            <td>18</td>
            <td>Drama</td>
        </tr>
        <tr>
            <td>27</td>
            <td>Horror</td>
        </tr>
        <tr>
            <td>28</td>
            <td>Action</td>
        </tr>
        <tr>
            <td>35</td>
            <td>Comedy</td>
        </tr>
        <tr>
            <td>36</td>
            <td>History</td>
        </tr>
        <tr>
            <td>37</td>
            <td>Western</td>
        </tr>
        <tr>
            <td>53</td>
            <td>Thriller</td>
        </tr>
        <tr>
            <td>80</td>
            <td>Crime</td>
        </tr>
        <tr>
            <td>99</td>
            <td>Documentary</td>
        </tr>
        <tr>
            <td>878</td>
            <td>Science Fiction</td>
        </tr>
        <tr>
            <td>9648</td>
            <td>Mystery</td>
        </tr>
        <tr>
            <td>10402</td>
            <td>Music</td>
        </tr>
        <tr>
            <td>10749</td>
            <td>Romance</td>
        </tr>
        <tr>
            <td>10751</td>
            <td>Family</td>
        </tr>
        <tr>
            <td>10752</td>
            <td>War</td>
        </tr>
        <tr>
            <td>10769</td>
            <td>Foreign</td>
        </tr>
        <tr>
            <td>10770</td>
            <td>TV Movie</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT m.movie_id,g.genre_name,m.title
        FROM movies AS m
    JOIN genremap AS gm
    ON m.movie_id=gm.movie_id
JOIN genres AS g
    ON g.genre_id=gm.genre_id
WHERE g.genre_name='Drama'
        GROUP BY  m.movie_id
    ORDER BY m.movie_id LIMIT 10;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>movie_id</th>
            <th>genre_name</th>
            <th>title</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>13</td>
            <td>Drama</td>
            <td>Forrest Gump</td>
        </tr>
        <tr>
            <td>14</td>
            <td>Drama</td>
            <td>American Beauty</td>
        </tr>
        <tr>
            <td>16</td>
            <td>Drama</td>
            <td>Dancer in the Dark</td>
        </tr>
        <tr>
            <td>19</td>
            <td>Drama</td>
            <td>Metropolis</td>
        </tr>
        <tr>
            <td>20</td>
            <td>Drama</td>
            <td>My Life Without Me</td>
        </tr>
        <tr>
            <td>25</td>
            <td>Drama</td>
            <td>Jarhead</td>
        </tr>
        <tr>
            <td>28</td>
            <td>Drama</td>
            <td>Apocalypse Now</td>
        </tr>
        <tr>
            <td>38</td>
            <td>Drama</td>
            <td>Eternal Sunshine of the Spotless Mind</td>
        </tr>
        <tr>
            <td>55</td>
            <td>Drama</td>
            <td>Amores perros</td>
        </tr>
        <tr>
            <td>59</td>
            <td>Drama</td>
            <td>A History of Violence</td>
        </tr>
    </tbody>
</table>



7.Find all actors who played in the movie ‘Inception’.


```sql
%%sql
SELECT m.title,a.actor_name,g.genre_name
FROM movies AS m
JOIN casts AS c
ON m.movie_id=c.movie_id    ----casts joins the actors to the movies table through the movie_id
JOIN actors AS a
ON c.actor_id=a.actor_id
JOIN genremap AS gm
ON gm.movie_id=c.movie_id
JOIN genres AS g
ON g.genre_id=gm.genre_id
WHERE title='Inception' LIMIT 10;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>actor_name</th>
            <th>genre_name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Inception</td>
            <td>Lukas Haas</td>
            <td>Adventure</td>
        </tr>
        <tr>
            <td>Inception</td>
            <td>Lukas Haas</td>
            <td>Action</td>
        </tr>
        <tr>
            <td>Inception</td>
            <td>Lukas Haas</td>
            <td>Thriller</td>
        </tr>
        <tr>
            <td>Inception</td>
            <td>Lukas Haas</td>
            <td>Science Fiction</td>
        </tr>
        <tr>
            <td>Inception</td>
            <td>Lukas Haas</td>
            <td>Mystery</td>
        </tr>
        <tr>
            <td>Inception</td>
            <td>Russ Fega</td>
            <td>Adventure</td>
        </tr>
        <tr>
            <td>Inception</td>
            <td>Russ Fega</td>
            <td>Action</td>
        </tr>
        <tr>
            <td>Inception</td>
            <td>Russ Fega</td>
            <td>Thriller</td>
        </tr>
        <tr>
            <td>Inception</td>
            <td>Russ Fega</td>
            <td>Science Fiction</td>
        </tr>
        <tr>
            <td>Inception</td>
            <td>Russ Fega</td>
            <td>Mystery</td>
        </tr>
    </tbody>
</table>



8.Show all movies released before the year 2000.


```sql
%%sql
SELECT film,name,year
FROM oscars
WHERE year<'2000' LIMIT 10;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>film</th>
            <th>name</th>
            <th>year</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>The Noose</td>
            <td>Richard Barthelmess</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>The Last Command</td>
            <td>Emil Jannings</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>A Ship Comes In</td>
            <td>Louise Dresser</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>7th Heaven</td>
            <td>Janet Gaynor</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>Sadie Thompson</td>
            <td>Gloria Swanson</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>Sunrise</td>
            <td>Rochus Gliese</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>The Dove; Tempest</td>
            <td>William Cameron Menzies</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>7th Heaven</td>
            <td>Harry Oliver</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>The Devil Dancer; The Magic Flame; Sadie Thompson</td>
            <td>George Barnes</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>Sunrise</td>
            <td>Charles Rosher</td>
            <td>1928</td>
        </tr>
    </tbody>
</table>



9.Show the role played by Leonardo DiCaprio in each movie.


```sql
%%sql 
PRAGMA table_info(oscars);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>year</td>
            <td>varchar(10)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>1</td>
            <td>award</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2</td>
            <td>winner</td>
            <td>varchar(10)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>3</td>
            <td>name</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>4</td>
            <td>film</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT name,award,film
FROM oscars
WHERE name='Leonardo DiCaprio';
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>name</th>
            <th>award</th>
            <th>film</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Leonardo DiCaprio</td>
            <td>Actor in a Supporting Role</td>
            <td>What&#x27;s Eating Gilbert Grape</td>
        </tr>
        <tr>
            <td>Leonardo DiCaprio</td>
            <td>Actor in a Leading Role</td>
            <td>The Aviator</td>
        </tr>
        <tr>
            <td>Leonardo DiCaprio</td>
            <td>Actor in a Leading Role</td>
            <td>Blood Diamond</td>
        </tr>
        <tr>
            <td>Leonardo DiCaprio</td>
            <td>Actor in a Leading Role</td>
            <td>The Wolf of Wall Street</td>
        </tr>
        <tr>
            <td>Leonardo DiCaprio</td>
            <td>Actor in a Leading Role</td>
            <td>The Revenant</td>
        </tr>
    </tbody>
</table>



10.Get a list of all genres in the database.


```sql
%%sql
SELECT *
FROM genres;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>genre_id</th>
            <th>genre_name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>12</td>
            <td>Adventure</td>
        </tr>
        <tr>
            <td>14</td>
            <td>Fantasy</td>
        </tr>
        <tr>
            <td>16</td>
            <td>Animation</td>
        </tr>
        <tr>
            <td>18</td>
            <td>Drama</td>
        </tr>
        <tr>
            <td>27</td>
            <td>Horror</td>
        </tr>
        <tr>
            <td>28</td>
            <td>Action</td>
        </tr>
        <tr>
            <td>35</td>
            <td>Comedy</td>
        </tr>
        <tr>
            <td>36</td>
            <td>History</td>
        </tr>
        <tr>
            <td>37</td>
            <td>Western</td>
        </tr>
        <tr>
            <td>53</td>
            <td>Thriller</td>
        </tr>
        <tr>
            <td>80</td>
            <td>Crime</td>
        </tr>
        <tr>
            <td>99</td>
            <td>Documentary</td>
        </tr>
        <tr>
            <td>878</td>
            <td>Science Fiction</td>
        </tr>
        <tr>
            <td>9648</td>
            <td>Mystery</td>
        </tr>
        <tr>
            <td>10402</td>
            <td>Music</td>
        </tr>
        <tr>
            <td>10749</td>
            <td>Romance</td>
        </tr>
        <tr>
            <td>10751</td>
            <td>Family</td>
        </tr>
        <tr>
            <td>10752</td>
            <td>War</td>
        </tr>
        <tr>
            <td>10769</td>
            <td>Foreign</td>
        </tr>
        <tr>
            <td>10770</td>
            <td>TV Movie</td>
        </tr>
    </tbody>
</table>



11.Count how many movies each genre has.


```sql
%%sql
SELECT g.genre_name,
COUNT(m.movie_id) AS Movie_count 
FROM movies AS m
JOIN genremap AS gm
ON gm.movie_id=m.movie_id    ---what connects genres to the movie table is the genremap table with the primary key movie_id
JOIN genres AS g
ON g.genre_id=gm.genre_id
GROUP BY g.genre_name
ORDER BY Movie_count DESC;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>genre_name</th>
            <th>Movie_count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Drama</td>
            <td>2297</td>
        </tr>
        <tr>
            <td>Comedy</td>
            <td>1722</td>
        </tr>
        <tr>
            <td>Thriller</td>
            <td>1274</td>
        </tr>
        <tr>
            <td>Action</td>
            <td>1154</td>
        </tr>
        <tr>
            <td>Romance</td>
            <td>894</td>
        </tr>
        <tr>
            <td>Adventure</td>
            <td>790</td>
        </tr>
        <tr>
            <td>Crime</td>
            <td>696</td>
        </tr>
        <tr>
            <td>Science Fiction</td>
            <td>535</td>
        </tr>
        <tr>
            <td>Horror</td>
            <td>519</td>
        </tr>
        <tr>
            <td>Family</td>
            <td>513</td>
        </tr>
        <tr>
            <td>Fantasy</td>
            <td>424</td>
        </tr>
        <tr>
            <td>Mystery</td>
            <td>348</td>
        </tr>
        <tr>
            <td>Animation</td>
            <td>234</td>
        </tr>
        <tr>
            <td>History</td>
            <td>197</td>
        </tr>
        <tr>
            <td>Music</td>
            <td>185</td>
        </tr>
        <tr>
            <td>War</td>
            <td>144</td>
        </tr>
        <tr>
            <td>Documentary</td>
            <td>110</td>
        </tr>
        <tr>
            <td>Western</td>
            <td>82</td>
        </tr>
        <tr>
            <td>Foreign</td>
            <td>34</td>
        </tr>
        <tr>
            <td>TV Movie</td>
            <td>8</td>
        </tr>
    </tbody>
</table>



12.List each movie with the number of actors in its cast.


```sql
%%sql
SELECT m.movie_id,a.actor_name,m.title,
COUNT(c.actor_id) AS No_of_actors
FROM movies AS m
JOIN casts AS c
ON c.movie_id=m.movie_id
JOIN actors AS a
ON a.actor_id=c.actor_id 
GROUP BY m.movie_id
ORDER BY No_of_actors DESC LIMIT 10;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>movie_id</th>
            <th>actor_name</th>
            <th>title</th>
            <th>No_of_actors</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>80585</td>
            <td>Tom Cruise</td>
            <td>Rock of Ages</td>
            <td>224</td>
        </tr>
        <tr>
            <td>3083</td>
            <td>James Stewart</td>
            <td>Mr. Smith Goes to Washington</td>
            <td>213</td>
        </tr>
        <tr>
            <td>324668</td>
            <td>Matt Damon</td>
            <td>Jason Bourne</td>
            <td>208</td>
        </tr>
        <tr>
            <td>82695</td>
            <td>Russell Crowe</td>
            <td>Les MisTrables</td>
            <td>208</td>
        </tr>
        <tr>
            <td>10661</td>
            <td>John Turturro</td>
            <td>You Don&#x27;t Mess with the Zohan</td>
            <td>183</td>
        </tr>
        <tr>
            <td>39254</td>
            <td>Hugh Jackman</td>
            <td>Real Steel</td>
            <td>172</td>
        </tr>
        <tr>
            <td>13475</td>
            <td>Lucia Rijker</td>
            <td>Star Trek</td>
            <td>168</td>
        </tr>
        <tr>
            <td>68728</td>
            <td>Michelle Williams</td>
            <td>Oz: The Great and Powerful</td>
            <td>159</td>
        </tr>
        <tr>
            <td>49026</td>
            <td>Gary Oldman</td>
            <td>The Dark Knight Rises</td>
            <td>158</td>
        </tr>
        <tr>
            <td>209112</td>
            <td>Michael Shannon</td>
            <td>Batman v Superman: Dawn of Justice</td>
            <td>152</td>
        </tr>
    </tbody>
</table>



13.Find all actors who have appeared in at least 3 movies.


```sql
%%sql
SELECT a.actor_name,m.title,m.movie_id,
COUNT(c.actor_id) AS No_of_actors
FROM movies AS m
JOIN casts AS c
ON c.movie_id=m.movie_id
JOIN actors AS a
ON a.actor_id=c.actor_id 
GROUP BY a.actor_id
HAVING(m.movie_id)>=3
ORDER BY No_of_actors DESC LIMIT 10;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>actor_name</th>
            <th>title</th>
            <th>movie_id</th>
            <th>No_of_actors</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Samuel L. Jackson</td>
            <td>Jackie Brown</td>
            <td>184</td>
            <td>67</td>
        </tr>
        <tr>
            <td>Robert De Niro</td>
            <td>Brazil</td>
            <td>68</td>
            <td>57</td>
        </tr>
        <tr>
            <td>Bruce Willis</td>
            <td>Four Rooms</td>
            <td>5</td>
            <td>51</td>
        </tr>
        <tr>
            <td>Matt Damon</td>
            <td>Ocean&#x27;s Eleven</td>
            <td>161</td>
            <td>48</td>
        </tr>
        <tr>
            <td>Morgan Freeman</td>
            <td>Unforgiven</td>
            <td>33</td>
            <td>46</td>
        </tr>
        <tr>
            <td>Steve Buscemi</td>
            <td>Armageddon</td>
            <td>95</td>
            <td>43</td>
        </tr>
        <tr>
            <td>Liam Neeson</td>
            <td>Batman Begins</td>
            <td>272</td>
            <td>41</td>
        </tr>
        <tr>
            <td>Owen Wilson</td>
            <td>Armageddon</td>
            <td>95</td>
            <td>40</td>
        </tr>
        <tr>
            <td>Johnny Depp</td>
            <td>Pirates of the Caribbean: The Curse of the Black Pearl</td>
            <td>22</td>
            <td>40</td>
        </tr>
        <tr>
            <td>Alec Baldwin</td>
            <td>Notting Hill</td>
            <td>509</td>
            <td>39</td>
        </tr>
    </tbody>
</table>



14.Show the total number of actors per genre (by joining Movies → Genres → Casts).


```sql
%%sql
SELECT a.actor_name,g.genre_name,
COUNT(c.actor_id) AS No_of_actors
FROM movies AS m
JOIN casts AS c
ON c.movie_id=m.movie_id
JOIN actors AS a
ON a.actor_id=c.actor_id 
JOIN genremap AS gm
ON gm.movie_id=m.movie_id
JOIN genres AS g 
ON g.genre_id=gm.genre_id
GROUP BY a.actor_id
ORDER BY No_of_actors DESC LIMIT 10;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>actor_name</th>
            <th>genre_name</th>
            <th>No_of_actors</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Samuel L. Jackson</td>
            <td>Adventure</td>
            <td>194</td>
        </tr>
        <tr>
            <td>Bruce Willis</td>
            <td>Adventure</td>
            <td>148</td>
        </tr>
        <tr>
            <td>Robert De Niro</td>
            <td>Adventure</td>
            <td>133</td>
        </tr>
        <tr>
            <td>Matt Damon</td>
            <td>Adventure</td>
            <td>131</td>
        </tr>
        <tr>
            <td>Frank Welker</td>
            <td>Adventure</td>
            <td>130</td>
        </tr>
        <tr>
            <td>Nicolas Cage</td>
            <td>Adventure</td>
            <td>126</td>
        </tr>
        <tr>
            <td>Morgan Freeman</td>
            <td>Adventure</td>
            <td>125</td>
        </tr>
        <tr>
            <td>Liam Neeson</td>
            <td>Adventure</td>
            <td>123</td>
        </tr>
        <tr>
            <td>Steve Buscemi</td>
            <td>Adventure</td>
            <td>115</td>
        </tr>
        <tr>
            <td>Owen Wilson</td>
            <td>Adventure</td>
            <td>111</td>
        </tr>
    </tbody>
</table>



15.Display all movies where Tom Hanks AND Meg Ryan was part of the cast.


```sql
%%sql
SELECT a.actor_name,m.title,m.movie_id
FROM movies AS m
JOIN casts AS c
ON c.movie_id=m.movie_id
JOIN actors AS a
ON a.actor_id=c.actor_id 
WHERE a.actor_name IN('Tom Hanks','Meg Ryan')
GROUP BY m.movie_id
ORDER BY m.movie_id DESC LIMIT 20;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>actor_name</th>
            <th>title</th>
            <th>movie_id</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Tom Hanks</td>
            <td>Bridge of Spies</td>
            <td>296098</td>
        </tr>
        <tr>
            <td>Tom Hanks</td>
            <td>Saving Mr. Banks</td>
            <td>140823</td>
        </tr>
        <tr>
            <td>Tom Hanks</td>
            <td>Captain Phillips</td>
            <td>109424</td>
        </tr>
        <tr>
            <td>Tom Hanks</td>
            <td>Cloud Atlas</td>
            <td>83542</td>
        </tr>
        <tr>
            <td>Tom Hanks</td>
            <td>Extremely Loud &amp; Incredibly Close</td>
            <td>64685</td>
        </tr>
        <tr>
            <td>Tom Hanks</td>
            <td>Larry Crowne</td>
            <td>59861</td>
        </tr>
        <tr>
            <td>Tom Hanks</td>
            <td>Radio Flyer</td>
            <td>20763</td>
        </tr>
        <tr>
            <td>Meg Ryan</td>
            <td>The Women</td>
            <td>13972</td>
        </tr>
        <tr>
            <td>Tom Hanks</td>
            <td>Who Killed the Electric Car?</td>
            <td>13508</td>
        </tr>
        <tr>
            <td>Tom Hanks</td>
            <td>Angels &amp; Demons</td>
            <td>13448</td>
        </tr>
        <tr>
            <td>Meg Ryan</td>
            <td>In the Land of Women</td>
            <td>13067</td>
        </tr>
        <tr>
            <td>Meg Ryan</td>
            <td>Proof of Life</td>
            <td>11983</td>
        </tr>
        <tr>
            <td>Tom Hanks</td>
            <td>A League of Their Own</td>
            <td>11287</td>
        </tr>
        <tr>
            <td>Meg Ryan</td>
            <td>Kate &amp; Leopold</td>
            <td>11232</td>
        </tr>
        <tr>
            <td>Meg Ryan</td>
            <td>In the Cut</td>
            <td>10944</td>
        </tr>
        <tr>
            <td>Meg Ryan</td>
            <td>Courage Under Fire</td>
            <td>10684</td>
        </tr>
        <tr>
            <td>Meg Ryan</td>
            <td>The Doors</td>
            <td>10537</td>
        </tr>
        <tr>
            <td>Meg Ryan</td>
            <td>Hanging Up</td>
            <td>10385</td>
        </tr>
        <tr>
            <td>Tom Hanks</td>
            <td>Toy Story 3</td>
            <td>10193</td>
        </tr>
        <tr>
            <td>Tom Hanks</td>
            <td>Philadelphia</td>
            <td>9800</td>
        </tr>
    </tbody>
</table>



16.Show the average rating for each movie.


```sql
%%sql
SELECT title,ROUND(AVG(popularity),2) AS AVG_popularity ----Round off to 2 decimals
FROM movies 
GROUP BY title
ORDER by AVG_popularity DESC LIMIT 7;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>AVG_popularity</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Minions</td>
            <td>875.58</td>
        </tr>
        <tr>
            <td>Interstellar</td>
            <td>724.25</td>
        </tr>
        <tr>
            <td>Deadpool</td>
            <td>514.57</td>
        </tr>
        <tr>
            <td>Guardians of the Galaxy</td>
            <td>481.1</td>
        </tr>
        <tr>
            <td>Mad Max: Fury Road</td>
            <td>434.28</td>
        </tr>
        <tr>
            <td>Jurassic World</td>
            <td>418.71</td>
        </tr>
        <tr>
            <td>Pirates of the Caribbean: The Curse of the Black Pearl</td>
            <td>271.97</td>
        </tr>
    </tbody>
</table>



17.Find all movies that have no ratings yet.


```sql
%%sql
SELECT title,ROUND(AVG(popularity),0) AS AVG_popularity
FROM movies
WHERE popularity IS NULL
GROUP BY title
ORDER by AVG_popularity DESC LIMIT 5;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>AVG_popularity</th>
        </tr>
    </thead>
    <tbody>
    </tbody>
</table>



18.FINDING MOVIES WHERE POPULARITY EQUALS TO ZERO


```sql
%%sql
SELECT title,overview,popularity
FROM movies
WHERE popularity=0.0;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>overview</th>
            <th>popularity</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>America Is Still the Place</td>
            <td>&quot;1971 post civil rights San Francisco seemed like the perfect place for a black Korean War veteran and his family to realize their dream of economic independence and his own chance to be his a &quot;&quot;boss&quot;&quot;. Charlie Walker would soon find out how naive he was. In a city full of impostors and naysayers, he refused to take &quot;&quot;No&quot;&quot; for an answer. Until a catastrophic disaster opened a door that had never been open to a black man before. This is a story about what happened when he stepped through that door, with both feet!.&quot;</td>
            <td>0.0</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT name, year
FROM oscars limit 5;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>name</th>
            <th>year</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Richard Barthelmess</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>Emil Jannings</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>Louise Dresser</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>Janet Gaynor</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>Gloria Swanson</td>
            <td>1928</td>
        </tr>
    </tbody>
</table>



19. Find movies and the views is not Null where popularity is greater than or equals to 300.


```sql
%%sql 
SELECT title,overview,popularity 
FROM movies 
WHERE popularity>='300'
AND overview IS NOT NULL
ORDER BY popularity DESC LIMIT 5;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>overview</th>
            <th>popularity</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Minions</td>
            <td>Minions Stuart, Kevin and Bob are recruited by Scarlet Overkill, a super-villain who, alongside her inventor husband Herb, hatches a plot to take over the world.</td>
            <td>875.581305</td>
        </tr>
        <tr>
            <td>Interstellar</td>
            <td>Interstellar chronicles the adventures of a group of explorers who make use of a newly discovered wormhole to surpass the limitations on human space travel and conquer the vast distances involved in an interstellar voyage.</td>
            <td>724.247784</td>
        </tr>
        <tr>
            <td>Deadpool</td>
            <td>Deadpool tells the origin story of former Special Forces operative turned mercenary Wade Wilson, who after being subjected to a rogue experiment that leaves him with accelerated healing powers, adopts the alter ego Deadpool. Armed with his new abilities and a dark, twisted sense of humor, Deadpool hunts down the man who nearly destroyed his life.</td>
            <td>514.569956</td>
        </tr>
        <tr>
            <td>Guardians of the Galaxy</td>
            <td>Light years from Earth, 26 years after being abducted, Peter Quill finds himself the prime target of a manhunt after discovering an orb wanted by Ronan the Accuser.</td>
            <td>481.098624</td>
        </tr>
        <tr>
            <td>Mad Max: Fury Road</td>
            <td>An apocalyptic story set in the furthest reaches of our planet, in a stark desert landscape where humanity is broken, and most everyone is crazed fighting for the necessities of life. Within this world exist two rebels on the run who just might be able to restore order. There&#x27;s Max, a man of action and a man of few words, who seeks peace of mind following the loss of his wife and child in the aftermath of the chaos. And Furiosa, a woman of action and a woman who believes her path to survival may be achieved if she can make it across the desert back to her childhood homeland.</td>
            <td>434.278564</td>
        </tr>
    </tbody>
</table>



20.Find the year when the movie American Beauty was produced


```sql
%%sql
SELECT title,release_date,release_status,popularity
FROM movies
WHERE title='American Beauty';
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>release_date</th>
            <th>release_status</th>
            <th>popularity</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>American Beauty</td>
            <td>1999-09-15 00:00:00.000000</td>
            <td>Released</td>
            <td>80.878605</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT DISTINCT year,title 
FROM oscars AS o
JOIN movies AS m 
ON c.movie_id=m.movie_id
JOIN casts AS c
ON m.movie_id=c.movie_id
WHERE title='American Beauty'LIMIT 5;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>year</th>
            <th>title</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1928</td>
            <td>American Beauty</td>
        </tr>
        <tr>
            <td>1929</td>
            <td>American Beauty</td>
        </tr>
        <tr>
            <td>1930</td>
            <td>American Beauty</td>
        </tr>
        <tr>
            <td>1931</td>
            <td>American Beauty</td>
        </tr>
        <tr>
            <td>1932</td>
            <td>American Beauty</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
PRAGMA table_info(movies);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>movie_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>title</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2</td>
            <td>release_date</td>
            <td>datetime(6)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>3</td>
            <td>budget</td>
            <td>INTEGER</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>4</td>
            <td>homepage</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>5</td>
            <td>original_language</td>
            <td>varchar(50)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>6</td>
            <td>original_title</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>7</td>
            <td>overview</td>
            <td>varchar(5000)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>8</td>
            <td>popularity</td>
            <td>double</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>9</td>
            <td>revenue</td>
            <td>double</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>10</td>
            <td>runtime</td>
            <td>double</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>11</td>
            <td>release_status</td>
            <td>varchar(50)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>12</td>
            <td>tagline</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>13</td>
            <td>vote_average</td>
            <td>double</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>14</td>
            <td>vote_count</td>
            <td>INTEGER</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
PRAGMA table_info(productioncompanies);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>production_company_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>production_company_name</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>



21.Using the Movies table, find the ReleaseYears that have more than 2 movies.


```sql
%%sql
SELECT title, release_date
  FROM Movies
  GROUP BY release_date
  HAVING COUNT(movie_id) > 2
 LIMIT 10;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>release_date</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Cat People</td>
            <td>1982-04-02 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Blade Runner</td>
            <td>1982-06-25 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Dune</td>
            <td>1984-12-14 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Creepshow 2</td>
            <td>1987-05-01 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Dances with Wolves</td>
            <td>1990-11-09 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Braveheart</td>
            <td>1995-05-24 00:00:00.000000</td>
        </tr>
        <tr>
            <td>The Net</td>
            <td>1995-07-28 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Leaving Las Vegas</td>
            <td>1995-10-27 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Dead Man Walking</td>
            <td>1995-12-29 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Eraser</td>
            <td>1996-06-21 00:00:00.000000</td>
        </tr>
    </tbody>
</table>



22.Title of each movie along with its production company, ordered alphabetically by Production company?


```sql
%%sql
SELECT m.title,pc.production_company_name,m.release_date
FROM movies AS m
INNER JOIN productioncompanymap AS pcm  ---productioncompanymap JOINS the productioncompanies table                                        
                                         ---to the movie table by movie_id column as PRIMARY KEY
ON m.movie_id=pcm.movie_id
LEFT JOIN productioncompanies AS pc
ON pc.production_company_id=pcm.production_company_id
ORDER BY pc.production_company_name limit 10;

```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>production_company_name</th>
            <th>release_date</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>A Man Apart</td>
            <td>&quot;&quot;&quot;DIA&quot;&quot; Productions GmbH &amp; Co. KG&quot;</td>
            <td>2003-04-04 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Downfall</td>
            <td>+sterreichischer Rundfunk (ORF)</td>
            <td>2004-09-08 00:00:00.000000</td>
        </tr>
        <tr>
            <td>The Second Mother</td>
            <td>-frica Filmes</td>
            <td>2015-02-08 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Rubber</td>
            <td>1.85 Films</td>
            <td>2010-07-09 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Do You Believe?</td>
            <td>10 West Studios</td>
            <td>2015-03-20 00:00:00.000000</td>
        </tr>
        <tr>
            <td>The Secret in Their Eyes</td>
            <td>100 Bares</td>
            <td>2009-08-13 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Underdogs</td>
            <td>100 Bares</td>
            <td>2013-07-18 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Captive</td>
            <td>1019 Entertainment</td>
            <td>2015-09-17 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Chairman of the Board</td>
            <td>101st Street Films</td>
            <td>1998-03-13 00:00:00.000000</td>
        </tr>
        <tr>
            <td>The Kids Are All Right</td>
            <td>10th Hole Productions</td>
            <td>2010-07-09 00:00:00.000000</td>
        </tr>
    </tbody>
</table>



23.SQL query correctly counts the total number of movies in the Movies table that have a Rating of 8.5 or higher?


```sql
%%sql 
SELECT COUNT(*)
FROM movies
WHERE popularity >=8.5;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>COUNT(*)</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2975</td>
        </tr>
    </tbody>
</table>



24.Write an SQL query that returns the Title and ReleaseYear of all movies released after the year 2010 from the Movies table.


```sql
%%sql
SELECT title,release_date
FROM
movies
WHERE release_date>2010 LIMIT 10;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>title</th>
            <th>release_date</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Four Rooms</td>
            <td>1995-12-09 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Star Wars</td>
            <td>1977-05-25 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Finding Nemo</td>
            <td>2003-05-30 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Forrest Gump</td>
            <td>1994-07-06 00:00:00.000000</td>
        </tr>
        <tr>
            <td>American Beauty</td>
            <td>1999-09-15 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Dancer in the Dark</td>
            <td>2000-05-17 00:00:00.000000</td>
        </tr>
        <tr>
            <td>The Fifth Element</td>
            <td>1997-05-07 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Metropolis</td>
            <td>1927-01-10 00:00:00.000000</td>
        </tr>
        <tr>
            <td>My Life Without Me</td>
            <td>2003-03-07 00:00:00.000000</td>
        </tr>
        <tr>
            <td>Pirates of the Caribbean: The Curse of the Black Pearl</td>
            <td>2003-07-09 00:00:00.000000</td>
        </tr>
    </tbody>
</table>



24.Find how many movies that where released in the year 1999


```sql
%%sql
SELECT COUNT(*) AS Movies_released_in_the_year_1999
FROM movies
WHERE release_date BETWEEN '1999-01-01' AND '1999-12-31';
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>Movies_released_in_the_year_1999</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>171</td>
        </tr>
    </tbody>
</table>



25.Find how many movies that where PRODUCED in the year 1999


```sql
%%sql
SELECT COUNT(DISTINCT film) AS PRODUCED_in_the_year_1999
FROM oscars
WHERE year='1999'
AND film IS NOT NULL;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>PRODUCED_in_the_year_1999</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>103</td>
        </tr>
    </tbody>
</table>



26.Show ALL movies that where PRODUCED in the year 1999


```sql
%%sql
SELECT DISTINCT film,year,name
FROM oscars
WHERE year='1999'
AND film IS NOT NULL;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>film</th>
            <th>year</th>
            <th>name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>The Insider</td>
            <td>1999</td>
            <td>Russell Crowe</td>
        </tr>
        <tr>
            <td>The Straight Story</td>
            <td>1999</td>
            <td>Richard Farnsworth</td>
        </tr>
        <tr>
            <td>Sweet and Lowdown</td>
            <td>1999</td>
            <td>Sean Penn</td>
        </tr>
        <tr>
            <td>American Beauty</td>
            <td>1999</td>
            <td>Kevin Spacey</td>
        </tr>
        <tr>
            <td>The Hurricane</td>
            <td>1999</td>
            <td>Denzel Washington</td>
        </tr>
        <tr>
            <td>The Cider House Rules</td>
            <td>1999</td>
            <td>Michael Caine</td>
        </tr>
        <tr>
            <td>Magnolia</td>
            <td>1999</td>
            <td>Tom Cruise</td>
        </tr>
        <tr>
            <td>The Green Mile</td>
            <td>1999</td>
            <td>Michael Clarke Duncan</td>
        </tr>
        <tr>
            <td>The Talented Mr. Ripley</td>
            <td>1999</td>
            <td>Jude Law</td>
        </tr>
        <tr>
            <td>The Sixth Sense</td>
            <td>1999</td>
            <td>Haley Joel Osment</td>
        </tr>
        <tr>
            <td>American Beauty</td>
            <td>1999</td>
            <td>Annette Bening</td>
        </tr>
        <tr>
            <td>Tumbleweeds</td>
            <td>1999</td>
            <td>Janet McTeer</td>
        </tr>
        <tr>
            <td>The End of the Affair</td>
            <td>1999</td>
            <td>Julianne Moore</td>
        </tr>
        <tr>
            <td>Music of the Heart</td>
            <td>1999</td>
            <td>Meryl Streep</td>
        </tr>
        <tr>
            <td>Boys Don&#x27;t Cry</td>
            <td>1999</td>
            <td>Hilary Swank</td>
        </tr>
        <tr>
            <td>The Sixth Sense</td>
            <td>1999</td>
            <td>Toni Collette</td>
        </tr>
        <tr>
            <td>Girl, Interrupted</td>
            <td>1999</td>
            <td>Angelina Jolie</td>
        </tr>
        <tr>
            <td>Being John Malkovich</td>
            <td>1999</td>
            <td>Catherine Keener</td>
        </tr>
        <tr>
            <td>Sweet and Lowdown</td>
            <td>1999</td>
            <td>Samantha Morton</td>
        </tr>
        <tr>
            <td>Boys Don&#x27;t Cry</td>
            <td>1999</td>
            <td>Chlo+½ Sevigny</td>
        </tr>
        <tr>
            <td>Art Direction: Luciana Arrighi; Set Decoration: Ian Whittaker</td>
            <td>1999</td>
            <td>Anna and the King </td>
        </tr>
        <tr>
            <td>Art Direction: David Gropman; Set Decoration: Beth Rubino</td>
            <td>1999</td>
            <td>The Cider House Rules </td>
        </tr>
        <tr>
            <td>Art Direction: Rick Heinrichs; Set Decoration: Peter Young</td>
            <td>1999</td>
            <td>Sleepy Hollow </td>
        </tr>
        <tr>
            <td>Art Direction: Roy Walker; Set Decoration: Bruno Cesari</td>
            <td>1999</td>
            <td>The Talented Mr. Ripley </td>
        </tr>
        <tr>
            <td>Art Direction: Eve Stewart; Set Decoration: Eve Stewart, John Bush</td>
            <td>1999</td>
            <td>Topsy-Turvy </td>
        </tr>
        <tr>
            <td>Conrad L. Hall</td>
            <td>1999</td>
            <td>American Beauty </td>
        </tr>
        <tr>
            <td>Roger Pratt</td>
            <td>1999</td>
            <td>The End of the Affair </td>
        </tr>
        <tr>
            <td>Dante Spinotti</td>
            <td>1999</td>
            <td>The Insider </td>
        </tr>
        <tr>
            <td>Emmanuel Lubezki</td>
            <td>1999</td>
            <td>Sleepy Hollow </td>
        </tr>
        <tr>
            <td>Robert Richardson</td>
            <td>1999</td>
            <td>Snow Falling on Cedars </td>
        </tr>
        <tr>
            <td>Jenny Beavan</td>
            <td>1999</td>
            <td>Anna and the King </td>
        </tr>
        <tr>
            <td>Colleen Atwood</td>
            <td>1999</td>
            <td>Sleepy Hollow </td>
        </tr>
        <tr>
            <td>Ann Roth, Gary Jones</td>
            <td>1999</td>
            <td>The Talented Mr. Ripley </td>
        </tr>
        <tr>
            <td>Milena Canonero</td>
            <td>1999</td>
            <td>Titus </td>
        </tr>
        <tr>
            <td>Lindy Hemming</td>
            <td>1999</td>
            <td>Topsy-Turvy </td>
        </tr>
        <tr>
            <td>Sam Mendes</td>
            <td>1999</td>
            <td>American Beauty </td>
        </tr>
        <tr>
            <td>Spike Jonze</td>
            <td>1999</td>
            <td>Being John Malkovich </td>
        </tr>
        <tr>
            <td>Lasse Hallstr+¦m</td>
            <td>1999</td>
            <td>The Cider House Rules </td>
        </tr>
        <tr>
            <td>Michael Mann</td>
            <td>1999</td>
            <td>The Insider </td>
        </tr>
        <tr>
            <td>M. Night Shyamalan</td>
            <td>1999</td>
            <td>The Sixth Sense </td>
        </tr>
        <tr>
            <td>Wim Wenders, Ulrich Felsberg</td>
            <td>1999</td>
            <td>Buena Vista Social Club </td>
        </tr>
        <tr>
            <td>Roko Belic, Adrian Belic</td>
            <td>1999</td>
            <td>Genghis Blues </td>
        </tr>
        <tr>
            <td>Nanette Burstein, Brett Morgen</td>
            <td>1999</td>
            <td>On the Ropes </td>
        </tr>
        <tr>
            <td>Arthur Cohn, Kevin Macdonald</td>
            <td>1999</td>
            <td>One Day in September </td>
        </tr>
        <tr>
            <td>Paola di Florio, Lilibet Foster</td>
            <td>1999</td>
            <td>Speaking in Strings </td>
        </tr>
        <tr>
            <td>Bert Van Bork</td>
            <td>1999</td>
            <td>Eyewitness </td>
        </tr>
        <tr>
            <td>Susan Hannah Hadary, William A. Whiteford</td>
            <td>1999</td>
            <td>King Gimp </td>
        </tr>
        <tr>
            <td>Simeon Soffer, Jonathan Stack</td>
            <td>1999</td>
            <td>The Wildest Show in the South: The Angola Prison Rodeo </td>
        </tr>
        <tr>
            <td>Tariq Anwar, Christopher Greenbury</td>
            <td>1999</td>
            <td>American Beauty </td>
        </tr>
        <tr>
            <td>Lisa Zeno Churgin</td>
            <td>1999</td>
            <td>The Cider House Rules </td>
        </tr>
        <tr>
            <td>William Goldenberg, Paul Rubell, David Rosenbloom</td>
            <td>1999</td>
            <td>The Insider </td>
        </tr>
        <tr>
            <td>Zach Staenberg</td>
            <td>1999</td>
            <td>The Matrix </td>
        </tr>
        <tr>
            <td>Andrew Mondshein</td>
            <td>1999</td>
            <td>The Sixth Sense </td>
        </tr>
        <tr>
            <td>Spain</td>
            <td>1999</td>
            <td>All about My Mother </td>
        </tr>
        <tr>
            <td>Nepal</td>
            <td>1999</td>
            <td>Caravan </td>
        </tr>
        <tr>
            <td>France</td>
            <td>1999</td>
            <td>East-West </td>
        </tr>
        <tr>
            <td>United Kingdom</td>
            <td>1999</td>
            <td>Solomon and Gaenor </td>
        </tr>
        <tr>
            <td>Sweden</td>
            <td>1999</td>
            <td>Under the Sun </td>
        </tr>
        <tr>
            <td>Mich+¿le Burke, Mike Smithson</td>
            <td>1999</td>
            <td>Austin Powers: The Spy Who Shagged Me </td>
        </tr>
        <tr>
            <td>Greg Cannom</td>
            <td>1999</td>
            <td>Bicentennial Man </td>
        </tr>
        <tr>
            <td>Rick Baker</td>
            <td>1999</td>
            <td>Life </td>
        </tr>
        <tr>
            <td>Christine Blundell, Trefor Proud</td>
            <td>1999</td>
            <td>Topsy-Turvy </td>
        </tr>
        <tr>
            <td>Thomas Newman</td>
            <td>1999</td>
            <td>American Beauty </td>
        </tr>
        <tr>
            <td>John Williams</td>
            <td>1999</td>
            <td>Angela&#x27;s Ashes </td>
        </tr>
        <tr>
            <td>Rachel Portman</td>
            <td>1999</td>
            <td>The Cider House Rules </td>
        </tr>
        <tr>
            <td>John Corigliano</td>
            <td>1999</td>
            <td>The Red Violin </td>
        </tr>
        <tr>
            <td>Gabriel Yared</td>
            <td>1999</td>
            <td>The Talented Mr. Ripley </td>
        </tr>
        <tr>
            <td>Music and Lyric by Trey Parker and Marc Shaiman</td>
            <td>1999</td>
            <td>&quot;&quot;&quot;Blame Canada&quot;&quot; from South Park: Bigger, Longer &amp; Uncut&quot;</td>
        </tr>
        <tr>
            <td>Music and Lyric by Diane Warren</td>
            <td>1999</td>
            <td>&quot;&quot;&quot;Music Of My Heart&quot;&quot; from Music of the Heart&quot;</td>
        </tr>
        <tr>
            <td>Music and Lyric by Aimee Mann</td>
            <td>1999</td>
            <td>&quot;&quot;&quot;Save Me&quot;&quot; from Magnolia&quot;</td>
        </tr>
        <tr>
            <td>Music and Lyric by Randy Newman</td>
            <td>1999</td>
            <td>&quot;&quot;&quot;When She Loved Me&quot;&quot; from Toy Story 2&quot;</td>
        </tr>
        <tr>
            <td>Music and Lyric by Phil Collins</td>
            <td>1999</td>
            <td>&quot;&quot;&quot;You&#x27;ll Be In My Heart&quot;&quot; from Tarzan&quot;</td>
        </tr>
        <tr>
            <td>Bruce Cohen and Dan Jinks, Producers</td>
            <td>1999</td>
            <td>American Beauty </td>
        </tr>
        <tr>
            <td>Richard N. Gladstein, Producer</td>
            <td>1999</td>
            <td>The Cider House Rules </td>
        </tr>
        <tr>
            <td>David Valdes and Frank Darabont, Producers</td>
            <td>1999</td>
            <td>The Green Mile </td>
        </tr>
        <tr>
            <td>Michael Mann and Pieter Jan Brugge, Producers</td>
            <td>1999</td>
            <td>The Insider </td>
        </tr>
        <tr>
            <td>Frank Marshall, Kathleen Kennedy and Barry Mendel, Producers</td>
            <td>1999</td>
            <td>The Sixth Sense </td>
        </tr>
        <tr>
            <td>Peter Peake</td>
            <td>1999</td>
            <td>Humdrum </td>
        </tr>
        <tr>
            <td>Torill Kove</td>
            <td>1999</td>
            <td>My Grandmother Ironed the King&#x27;s Shirts </td>
        </tr>
        <tr>
            <td>Alexander Petrov</td>
            <td>1999</td>
            <td>The Old Man and the Sea </td>
        </tr>
        <tr>
            <td>Paul Driessen</td>
            <td>1999</td>
            <td>3 Misses </td>
        </tr>
        <tr>
            <td>Wendy Tilby, Amanda Forbis</td>
            <td>1999</td>
            <td>When the Day Breaks </td>
        </tr>
        <tr>
            <td>Henrik Ruben Genz, Michael W. Horsten</td>
            <td>1999</td>
            <td>Bror, Min Bror (Teis and Nico) </td>
        </tr>
        <tr>
            <td>Mehdi Norowzian, Steve Wax</td>
            <td>1999</td>
            <td>Killing Joe </td>
        </tr>
        <tr>
            <td>Marc-Andreas Bochert, Gabriele Lins</td>
            <td>1999</td>
            <td>Kleingeld (Small Change) </td>
        </tr>
        <tr>
            <td>Marcus Olsson</td>
            <td>1999</td>
            <td>Major and Minor Miracles </td>
        </tr>
        <tr>
            <td>Barbara Schock, Tammy Tiehel</td>
            <td>1999</td>
            <td>My Mother Dreams the Satan&#x27;s Disciples in New York </td>
        </tr>
        <tr>
            <td>Robert J. Litt, Elliot Tyson, Michael Herbick, Willie D. Burton</td>
            <td>1999</td>
            <td>The Green Mile </td>
        </tr>
        <tr>
            <td>Andy Nelson, Doug Hemphill, Lee Orloff</td>
            <td>1999</td>
            <td>The Insider </td>
        </tr>
        <tr>
            <td>John Reitz, Gregg Rudloff, David Campbell, David Lee</td>
            <td>1999</td>
            <td>The Matrix </td>
        </tr>
        <tr>
            <td>Leslie Shatz, Chris Carpenter, Rick Kline, Chris Munro</td>
            <td>1999</td>
            <td>The Mummy </td>
        </tr>
        <tr>
            <td>Gary Rydstrom, Tom Johnson, Shawn Murphy, John Midgley</td>
            <td>1999</td>
            <td>Star Wars Episode I: The Phantom Menace </td>
        </tr>
        <tr>
            <td>Ren Klyce, Richard Hymns</td>
            <td>1999</td>
            <td>Fight Club </td>
        </tr>
        <tr>
            <td>Dane A. Davis</td>
            <td>1999</td>
            <td>The Matrix </td>
        </tr>
        <tr>
            <td>Ben Burtt, Tom Bellfort</td>
            <td>1999</td>
            <td>Star Wars Episode I: The Phantom Menace </td>
        </tr>
        <tr>
            <td>John Gaeta, Janek Sirrs, Steve Courtley, Jon Thum</td>
            <td>1999</td>
            <td>The Matrix </td>
        </tr>
        <tr>
            <td>John Knoll, Dennis Muren, Scott Squires, Rob Coleman</td>
            <td>1999</td>
            <td>Star Wars Episode I: The Phantom Menace </td>
        </tr>
        <tr>
            <td>John Dykstra, Jerome Chen, Henry F. Anderson III, Eric Allard</td>
            <td>1999</td>
            <td>Stuart Little </td>
        </tr>
        <tr>
            <td>John Irving</td>
            <td>1999</td>
            <td>The Cider House Rules </td>
        </tr>
        <tr>
            <td>Alexander Payne, Jim Taylor</td>
            <td>1999</td>
            <td>Election </td>
        </tr>
        <tr>
            <td>Frank Darabont</td>
            <td>1999</td>
            <td>The Green Mile </td>
        </tr>
        <tr>
            <td>Eric Roth, Michael Mann</td>
            <td>1999</td>
            <td>The Insider </td>
        </tr>
        <tr>
            <td>Anthony Minghella</td>
            <td>1999</td>
            <td>The Talented Mr. Ripley </td>
        </tr>
        <tr>
            <td>Alan Ball</td>
            <td>1999</td>
            <td>American Beauty </td>
        </tr>
        <tr>
            <td>Charlie Kaufman</td>
            <td>1999</td>
            <td>Being John Malkovich </td>
        </tr>
        <tr>
            <td>Paul Thomas Anderson</td>
            <td>1999</td>
            <td>Magnolia </td>
        </tr>
        <tr>
            <td>Mike Leigh</td>
            <td>1999</td>
            <td>Topsy-Turvy </td>
        </tr>
    </tbody>
</table>



27.Find how many movies where produced before 1998


```sql
%%sql
SELECT COUNT(*) AS movies_produced_Before_1998
FROM oscars
WHERE year<'1998'
AND film IS NOT NULL;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>movies_produced_Before_1998</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>7570</td>
        </tr>
    </tbody>
</table>



28.show movies produced before 1998


```sql
%%sql
SELECT year,film
FROM oscars
WHERE year <'1998' 
AND film IS NOT NULL
ORDER BY year DESC LIMIT 5;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>year</th>
            <th>film</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1997</td>
            <td>Good Will Hunting</td>
        </tr>
        <tr>
            <td>1997</td>
            <td>The Apostle</td>
        </tr>
        <tr>
            <td>1997</td>
            <td>Ulee&#x27;s Gold</td>
        </tr>
        <tr>
            <td>1997</td>
            <td>Wag the Dog</td>
        </tr>
        <tr>
            <td>1997</td>
            <td>As Good as It Gets</td>
        </tr>
    </tbody>
</table>



29.Find the actor in leading role in 2015 winner is 1


```sql
%%sql
SELECT
name,award,film,year
FROM 
oscars
WHERE award='Actor in a Leading Role'
AND year='2015'
AND winner='1.0';
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>name</th>
            <th>award</th>
            <th>film</th>
            <th>year</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Leonardo DiCaprio</td>
            <td>Actor in a Leading Role</td>
            <td>The Revenant</td>
            <td>2015</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT year,name,award,film
FROM oscars
WHERE award='Actor in a Supporting Role'
AND year='2015'
AND winner='1.0';
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>year</th>
            <th>name</th>
            <th>award</th>
            <th>film</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2015</td>
            <td>Mark Rylance</td>
            <td>Actor in a Supporting Role</td>
            <td>Bridge of Spies</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT year,name,award,film
FROM oscars
WHERE award='Actress in a Leading Role'
AND year='2015'
AND winner='1.0';
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>year</th>
            <th>name</th>
            <th>award</th>
            <th>film</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2015</td>
            <td>Brie Larson</td>
            <td>Actress in a Leading Role</td>
            <td>Room</td>
        </tr>
    </tbody>
</table>



Find the Julianne Moore movies,role and film where year is 1928


```sql
%%sql
SELECT DISTINCT o.film,a.actor_name,m.title,o.award,o.year
FROM oscars AS o
JOIN movies AS m
ON m.movie_id=c.movie_id
JOIN casts AS c
ON c.movie_id=m.movie_id
JOIN actors AS a
ON a.actor_id=c.actor_id
WHERE a.actor_name='Julianne Moore'
AND o.year='1928'LIMIT 20;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>film</th>
            <th>actor_name</th>
            <th>title</th>
            <th>award</th>
            <th>year</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>The Last Command</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Actor</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Actor</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>7th Heaven</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Actress</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>A Ship Comes In</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Actress</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>Sadie Thompson</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Actress</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>7th Heaven</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Art Direction</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>Sunrise</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Art Direction</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>The Dove; Tempest</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Art Direction</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>Sunrise</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Cinematography</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>The Devil Dancer; The Magic Flame; Sadie Thompson</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Cinematography</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>Speedy</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Directing (Comedy Picture)</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>Two Arabian Knights</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Directing (Comedy Picture)</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>7th Heaven</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Directing (Dramatic Picture)</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>Sorrell and Son</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Directing (Dramatic Picture)</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>The Crowd</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Directing (Dramatic Picture)</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>None</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Engineering Effects</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>Wings</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Engineering Effects</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>7th Heaven</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Outstanding Picture</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>The Racket</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Outstanding Picture</td>
            <td>1928</td>
        </tr>
        <tr>
            <td>Wings</td>
            <td>Julianne Moore</td>
            <td>The Big Lebowski</td>
            <td>Outstanding Picture</td>
            <td>1928</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT DISTINCT o.film,a.actor_name,a.actor_id,m.title,o.award
FROM oscars AS o
JOIN movies AS m
ON m.movie_id=c.movie_id
JOIN casts AS c
ON c.movie_id=m.movie_id
JOIN actors AS a
ON a.actor_id=c.actor_id
WHERE m.title='Annie Hall' limit 20;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>film</th>
            <th>actor_name</th>
            <th>actor_id</th>
            <th>title</th>
            <th>award</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>The Noose</td>
            <td>Paula Trueman</td>
            <td>734</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>Beverly D&#x27;Angelo</td>
            <td>821</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>Woody Allen</td>
            <td>1243</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>Mark Lenard</td>
            <td>1820</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>Truman Capote</td>
            <td>1930</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>Diane Keaton</td>
            <td>3092</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>Shelley Hack</td>
            <td>3665</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>Tracey Walter</td>
            <td>3801</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>Christopher Walken</td>
            <td>4690</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>Jeff Goldblum</td>
            <td>4785</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>John Glover</td>
            <td>5589</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>Walter Bernstein</td>
            <td>6835</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>Sigourney Weaver</td>
            <td>10205</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>Shelley Duvall</td>
            <td>10409</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>Tony Roberts</td>
            <td>10555</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>Carol Kane</td>
            <td>10556</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>Paul Simon</td>
            <td>10557</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>Janet Margolin</td>
            <td>10558</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>Colleen Dewhurst</td>
            <td>10559</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
        <tr>
            <td>The Noose</td>
            <td>Donald Symington</td>
            <td>10560</td>
            <td>Annie Hall</td>
            <td>Actor</td>
        </tr>
    </tbody>
</table>




```sql
%%sql 
PRAGMA table_info(actors);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>actor_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>actor_name</td>
            <td>varchar(100)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2</td>
            <td>gender</td>
            <td>INTEGER</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>




```sql
%%sql 
PRAGMA table_info(casts);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>movie_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>actor_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>2</td>
        </tr>
        <tr>
            <td>2</td>
            <td>characters</td>
            <td>varchar(500)</td>
            <td>1</td>
            <td>None</td>
            <td>3</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
PRAGMA table_info(productioncompanies);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>production_company_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>production_company_name</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
PRAGMA table_info(movies);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>movie_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>title</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2</td>
            <td>release_date</td>
            <td>datetime(6)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>3</td>
            <td>budget</td>
            <td>INTEGER</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>4</td>
            <td>homepage</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>5</td>
            <td>original_language</td>
            <td>varchar(50)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>6</td>
            <td>original_title</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>7</td>
            <td>overview</td>
            <td>varchar(5000)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>8</td>
            <td>popularity</td>
            <td>double</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>9</td>
            <td>revenue</td>
            <td>double</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>10</td>
            <td>runtime</td>
            <td>double</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>11</td>
            <td>release_status</td>
            <td>varchar(50)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>12</td>
            <td>tagline</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>13</td>
            <td>vote_average</td>
            <td>double</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>14</td>
            <td>vote_count</td>
            <td>INTEGER</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT year,name, film
FROM oscars
WHERE award = 'Actor in a Leading Role'
  AND year = '2015'
  AND winner = '1.0';
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>year</th>
            <th>name</th>
            <th>film</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2015</td>
            <td>Leonardo DiCaprio</td>
            <td>The Revenant</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT * 
FROM movies
WHERE release_date IS NOT NULL
ORDER BY release_date ASC
LIMIT 2;

```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>movie_id</th>
            <th>title</th>
            <th>release_date</th>
            <th>budget</th>
            <th>homepage</th>
            <th>original_language</th>
            <th>original_title</th>
            <th>overview</th>
            <th>popularity</th>
            <th>revenue</th>
            <th>runtime</th>
            <th>release_status</th>
            <th>tagline</th>
            <th>vote_average</th>
            <th>vote_count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>3059</td>
            <td>Intolerance</td>
            <td>1916-09-04 00:00:00.000000</td>
            <td>385907</td>
            <td>None</td>
            <td>en</td>
            <td>Intolerance</td>
            <td>The story of a poor young woman, separated by prejudice from her husband and baby, is interwoven with tales of intolerance from throughout history.</td>
            <td>3.232447</td>
            <td>8394751.0</td>
            <td>197.0</td>
            <td>Released</td>
            <td>The Cruel Hand of Intolerance</td>
            <td>7.4</td>
            <td>60</td>
        </tr>
        <tr>
            <td>3060</td>
            <td>The Big Parade</td>
            <td>1925-11-05 00:00:00.000000</td>
            <td>245000</td>
            <td>None</td>
            <td>en</td>
            <td>The Big Parade</td>
            <td>The story of an idle rich boy who joins the US Army&#x27;s Rainbow Division and is sent to France to fight in World War I, becomes friends with two working class men, experiences the horrors of trench warfare, and finds love with a French girl.</td>
            <td>0.785744</td>
            <td>22000000.0</td>
            <td>151.0</td>
            <td>Released</td>
            <td>None</td>
            <td>7.0</td>
            <td>21</td>
        </tr>
    </tbody>
</table>



 Question 30

How many unique awards are there in the Oscars table?

**Options:**
 - 141
 - 53 
 - 80
 - 114


```sql
%%sql
SELECT 
COUNT(DISTINCT award) AS unique_awards
FROM oscars;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>unique_awards</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>114</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
PRAGMA table_info(oscars);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>year</td>
            <td>varchar(10)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>1</td>
            <td>award</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2</td>
            <td>winner</td>
            <td>varchar(10)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>3</td>
            <td>name</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>4</td>
            <td>film</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>



Question 31

How many movies are there that contain the word “Spider” within their title? WE use the movies table column title

**Options:**
 - 0
 - 5
 - 1
 - 9

Cheking the Movies with the word Spider and we use COUNT ALL FROM THE TABLE MOVIES AND USE THE LIKE TO DEFINE what we want to find.


```sql
%%sql
SELECT COUNT(*) AS Spider_movies
FROM movies
WHERE title LIKE '%Spider%';

```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>Spider_movies</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>9</td>
        </tr>
    </tbody>
</table>



Question 32

How many movies are there that are both in the "Thriller" genre and contain the word “love” anywhere in the keywords?


**Options:**
 - 48
 - 38
 - 14
 - 1


```sql
%%sql
PRAGMA table_info(keywordmap);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>movie_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>keyword_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>2</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
PRAGMA table_info(keywords);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>keyword_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>keyword_name</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
PRAGMA table_info(genres);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>genre_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>genre_name</td>
            <td>varchar(50)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
PRAGMA table_info(genremap);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>movie_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>genre_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>2</td>
        </tr>
    </tbody>
</table>




```python
%reload_ext sql
%sql sqlite:///TMDB-a-4006.db
```


```python
'Connected: @TMDB-a-4006.db'
```




    'Connected: @TMDB-a-4006.db'



How many movies are there that are both in the "Thriller" genre and contain the word “love” anywhere in the keywords?


```sql
%%sql
SELECT g.genre_name,k.keyword_name ,
COUNT(DISTINCT m.movie_id) AS thriller_love_movies
FROM movies AS m
JOIN genremap AS gm
    ON gm.movie_id = m.movie_id
JOIN genres AS g
    ON g.genre_id = gm.genre_id
JOIN keywordmap AS km
    ON km.movie_id = m.movie_id
JOIN keywords AS k
    ON k.keyword_id = km.keyword_id
WHERE g.genre_name = 'Thriller'
  AND k.keyword_name LIKE '%love%';
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>genre_name</th>
            <th>keyword_name</th>
            <th>thriller_love_movies</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Thriller</td>
            <td>love</td>
            <td>48</td>
        </tr>
    </tbody>
</table>



Question 35

How many movies are there that were released between 1 August 2006 ('2006-08-01') and 1 October 2009 ('2009-10-01') that have a popularity score of more than 40 and a budget of less than 50 000 000?

 
**Options:**

 - 29
 - 23
 - 28
 - 35


```sql
%%sql
SELECT COUNT(*) AS movie_count
FROM movies
WHERE release_date BETWEEN '2006-08-01' AND '2009-10-01'
  AND popularity > 40
  AND budget < 50000000;

```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>movie_count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>29</td>
        </tr>
    </tbody>
</table>



36.Show all movies released between 2010 and 2020, along with their production companies


```sql
%%sql
SELECT m.movie_id,m.title,m.release_date,pc.production_company_name
FROM movies AS m
LEFT JOIN productioncompanymap AS pcm
ON pcm.movie_id=m.movie_id
JOIN productioncompanies AS pc
ON pc.production_company_id=pcm.production_company_id
WHERE release_date BETWEEN '2010-01-01' AND '2020-12-31' 
LIMIT 10;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>movie_id</th>
            <th>title</th>
            <th>release_date</th>
            <th>production_company_name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>72431</td>
            <td>Red Tails</td>
            <td>2012-01-19 00:00:00.000000</td>
            <td>Lucasfilm</td>
        </tr>
        <tr>
            <td>1865</td>
            <td>Pirates of the Caribbean: On Stranger Tides</td>
            <td>2011-05-14 00:00:00.000000</td>
            <td>Walt Disney Pictures</td>
        </tr>
        <tr>
            <td>9543</td>
            <td>Prince of Persia: The Sands of Time</td>
            <td>2010-05-19 00:00:00.000000</td>
            <td>Walt Disney Pictures</td>
        </tr>
        <tr>
            <td>10193</td>
            <td>Toy Story 3</td>
            <td>2010-06-16 00:00:00.000000</td>
            <td>Walt Disney Pictures</td>
        </tr>
        <tr>
            <td>12155</td>
            <td>Alice in Wonderland</td>
            <td>2010-03-03 00:00:00.000000</td>
            <td>Walt Disney Pictures</td>
        </tr>
        <tr>
            <td>20526</td>
            <td>TRON: Legacy</td>
            <td>2010-12-10 00:00:00.000000</td>
            <td>Walt Disney Pictures</td>
        </tr>
        <tr>
            <td>27022</td>
            <td>The Sorcerer&#x27;s Apprentice</td>
            <td>2010-07-13 00:00:00.000000</td>
            <td>Walt Disney Pictures</td>
        </tr>
        <tr>
            <td>38757</td>
            <td>Tangled</td>
            <td>2010-11-24 00:00:00.000000</td>
            <td>Walt Disney Pictures</td>
        </tr>
        <tr>
            <td>39486</td>
            <td>Secretariat</td>
            <td>2010-08-20 00:00:00.000000</td>
            <td>Walt Disney Pictures</td>
        </tr>
        <tr>
            <td>49013</td>
            <td>Cars 2</td>
            <td>2011-06-11 00:00:00.000000</td>
            <td>Walt Disney Pictures</td>
        </tr>
    </tbody>
</table>



Question 37

How many unique characters has "Vin Diesel" played so far in the database?

**Options:**
 - 24
 - 19
 - 18
 - 16


```sql
%%sql
PRAGMA table_info(actors);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>actor_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>actor_name</td>
            <td>varchar(100)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2</td>
            <td>gender</td>
            <td>INTEGER</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
PRAGMA table_info(casts);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>movie_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>actor_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>2</td>
        </tr>
        <tr>
            <td>2</td>
            <td>characters</td>
            <td>varchar(500)</td>
            <td>1</td>
            <td>None</td>
            <td>3</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT a.actor_id,a.actor_name,
COUNT(DISTINCT c.characters) AS VIN_character 
FROM actors AS a
JOIN casts AS c
ON c.actor_id = a.actor_id
WHERE a.actor_name = 'Vin Diesel';

```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>actor_id</th>
            <th>actor_name</th>
            <th>VIN_character</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>12835</td>
            <td>Vin Diesel</td>
            <td>16</td>
        </tr>
    </tbody>
</table>



Question 38 What are the genres of the movie “The Royal Tenenbaums”?
Options:
 - Action, Romance
 - Drama, Comedy
 - Crime, Thriller
 - Drama, Romance


```sql
%%sql
PRAGMA table_info(movies);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>movie_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>title</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2</td>
            <td>release_date</td>
            <td>datetime(6)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>3</td>
            <td>budget</td>
            <td>INTEGER</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>4</td>
            <td>homepage</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>5</td>
            <td>original_language</td>
            <td>varchar(50)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>6</td>
            <td>original_title</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>7</td>
            <td>overview</td>
            <td>varchar(5000)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>8</td>
            <td>popularity</td>
            <td>double</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>9</td>
            <td>revenue</td>
            <td>double</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>10</td>
            <td>runtime</td>
            <td>double</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>11</td>
            <td>release_status</td>
            <td>varchar(50)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>12</td>
            <td>tagline</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>13</td>
            <td>vote_average</td>
            <td>double</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>14</td>
            <td>vote_count</td>
            <td>INTEGER</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
PRAGMA table_info(genremap);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>movie_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>genre_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>2</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
PRAGMA table_info(genres);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>genre_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>genre_name</td>
            <td>varchar(50)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT gm.movie_id,m.title,g.genre_name ---note you join tables with common primary key 
FROM movies AS m
JOIN genremap AS gm
ON m.movie_id=gm.movie_id
JOIN genres AS g
ON g.genre_id=gm.genre_id
WHERE m.title = 'The Royal Tenenbaums';

```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>movie_id</th>
            <th>title</th>
            <th>genre_name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>9428</td>
            <td>The Royal Tenenbaums</td>
            <td>Drama</td>
        </tr>
        <tr>
            <td>9428</td>
            <td>The Royal Tenenbaums</td>
            <td>Comedy</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
PRAGMA table_info(productioncompanymap);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>movie_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>production_company_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>2</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
PRAGMA table_info(productioncompanies);

```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>production_company_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>production_company_name</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>



Question 39

What are the three production companies that have the highest movie popularity score on average, as recorded within the database?
Options:
 - MCL Films S.A., Turner Pictures, and George Stevens Productions
 - The Donners' Company, Bulletproof Cupid, and Kinberg Genre
 - Bulletproof Cupid, The Donners' Company, and MCL Films S.A
 - B.Sting Entertainment, Illumination Pictures, and Aztec Musique


```sql
%%sql
SELECT pc.production_company_id,pc.production_company_name,pcm.movie_id,m.movie_id,
ROUND(AVG(m.popularity),1) AS AVG_popularity
FROM movies AS m
JOIN productioncompanymap AS pcm
ON m.movie_id=pcm.movie_id
JOIN productioncompanies AS pc
ON pc.production_company_id=pcm.production_company_id
GROUP BY pc.production_company_name
ORDER BY AVG_popularity DESC
limit 5;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>production_company_id</th>
            <th>production_company_name</th>
            <th>movie_id</th>
            <th>movie_id_1</th>
            <th>AVG_popularity</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>11307</td>
            <td>The Donners&#x27; Company</td>
            <td>293660</td>
            <td>293660</td>
            <td>514.6</td>
        </tr>
        <tr>
            <td>54850</td>
            <td>Bulletproof Cupid</td>
            <td>118340</td>
            <td>118340</td>
            <td>481.1</td>
        </tr>
        <tr>
            <td>78091</td>
            <td>Kinberg Genre</td>
            <td>246655</td>
            <td>246655</td>
            <td>326.9</td>
        </tr>
        <tr>
            <td>6704</td>
            <td>Illumination Entertainment</td>
            <td>20352</td>
            <td>20352</td>
            <td>234.9</td>
        </tr>
        <tr>
            <td>84424</td>
            <td>Vita-Ray Dutch Productions (III)</td>
            <td>271110</td>
            <td>271110</td>
            <td>198.4</td>
        </tr>
    </tbody>
</table>



40.Show the production companies for movie Shutter and the release_date 


```sql
%%sql
SELECT
pc.production_company_name,m.title,m.release_date,pcm.movie_id
FROM movies AS m
JOIN productioncompanymap AS pcm
ON m.movie_id=pcm.movie_id
JOIN productioncompanies AS pc
ON pc.production_company_id=pcm.production_company_id
WHERE m.title='Shutter';
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>production_company_name</th>
            <th>title</th>
            <th>release_date</th>
            <th>movie_id</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Regency Enterprises</td>
            <td>Shutter</td>
            <td>2008-03-21 00:00:00.000000</td>
            <td>10885</td>
        </tr>
        <tr>
            <td>Vertigo Entertainment</td>
            <td>Shutter</td>
            <td>2008-03-21 00:00:00.000000</td>
            <td>10885</td>
        </tr>
        <tr>
            <td>Ozla Pictures</td>
            <td>Shutter</td>
            <td>2008-03-21 00:00:00.000000</td>
            <td>10885</td>
        </tr>
        <tr>
            <td>New Regency Pictures</td>
            <td>Shutter</td>
            <td>2008-03-21 00:00:00.000000</td>
            <td>10885</td>
        </tr>
    </tbody>
</table>



41.Show the production companies,movie id for movie Eyes Wide Shut and the release_date


```sql
%%sql
SELECT
pc.production_company_id,pc.production_company_name,m.title,m.release_date,pcm.movie_id
FROM movies AS m
JOIN productioncompanymap AS pcm
ON m.movie_id=pcm.movie_id
JOIN productioncompanies AS pc
ON pc.production_company_id=pcm.production_company_id
WHERE m.title='Eyes Wide Shut';
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>production_company_id</th>
            <th>production_company_name</th>
            <th>title</th>
            <th>release_date</th>
            <th>movie_id</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>194</td>
            <td>Hobby Films</td>
            <td>Eyes Wide Shut</td>
            <td>1999-07-14 00:00:00.000000</td>
            <td>345</td>
        </tr>
        <tr>
            <td>195</td>
            <td>Pole Star</td>
            <td>Eyes Wide Shut</td>
            <td>1999-07-14 00:00:00.000000</td>
            <td>345</td>
        </tr>
        <tr>
            <td>385</td>
            <td>Stanley Kubrick Productions</td>
            <td>Eyes Wide Shut</td>
            <td>1999-07-14 00:00:00.000000</td>
            <td>345</td>
        </tr>
        <tr>
            <td>6194</td>
            <td>Warner Bros.</td>
            <td>Eyes Wide Shut</td>
            <td>1999-07-14 00:00:00.000000</td>
            <td>345</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
PRAGMA table_info(sysdiagrams);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>name</td>
            <td>varchar(160)</td>
            <td>1</td>
            <td>None</td>
            <td>0</td>
        </tr>
        <tr>
            <td>1</td>
            <td>principal_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2</td>
            <td>diagram_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>3</td>
            <td>version</td>
            <td>INTEGER</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>4</td>
            <td>definition</td>
            <td>longblob</td>
            <td>0</td>
            <td>None</td>
            <td>0</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT name,principal_id 
FROM sysdiagrams;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>name</th>
            <th>principal_id</th>
        </tr>
    </thead>
    <tbody>
    </tbody>
</table>



Question 42
How many female actors (i.e. gender = 1) have a name that starts with the letter "N"?
Options:
 - 0
 - 355
 - 7335
 - 1949


```sql
%%sql
PRAGMA table_info(casts);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>movie_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>actor_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>2</td>
        </tr>
        <tr>
            <td>2</td>
            <td>characters</td>
            <td>varchar(500)</td>
            <td>1</td>
            <td>None</td>
            <td>3</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
PRAGMA table_info(actors);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>actor_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>actor_name</td>
            <td>varchar(100)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2</td>
            <td>gender</td>
            <td>INTEGER</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>




```python
%sql sqlite:///TMDB-a-4006.db

```


```python
'Connected: @TMDB-a-4006.db'
```




    'Connected: @TMDB-a-4006.db'




```sql
%%sql
SELECT * FROM actors LIMIT 10;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>actor_id</th>
            <th>actor_name</th>
            <th>gender</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td>
            <td>George Lucas</td>
            <td>2</td>
        </tr>
        <tr>
            <td>2</td>
            <td>Mark Hamill</td>
            <td>2</td>
        </tr>
        <tr>
            <td>3</td>
            <td>Harrison Ford</td>
            <td>2</td>
        </tr>
        <tr>
            <td>4</td>
            <td>Carrie Fisher</td>
            <td>1</td>
        </tr>
        <tr>
            <td>5</td>
            <td>Peter Cushing</td>
            <td>2</td>
        </tr>
        <tr>
            <td>6</td>
            <td>Anthony Daniels</td>
            <td>2</td>
        </tr>
        <tr>
            <td>7</td>
            <td>Andrew Stanton</td>
            <td>2</td>
        </tr>
        <tr>
            <td>8</td>
            <td>Lee Unkrich</td>
            <td>2</td>
        </tr>
        <tr>
            <td>10</td>
            <td>Bob Peterson</td>
            <td>2</td>
        </tr>
        <tr>
            <td>11</td>
            <td>David Reynolds</td>
            <td>2</td>
        </tr>
    </tbody>
</table>



How many female actors (i.e. gender = 1) have a name that starts with the letter "N"?


```sql
%%sql
SELECT COUNT(*) AS Male_Actors_starting_with_K
FROM actors
WHERE gender= 2
AND actor_name LIKE 'K%';
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>Male_Actors_starting_with_K</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>556</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT COUNT(*) AS female_actors_starting_with_N
FROM actors
WHERE gender = 1
AND actor_name LIKE 'N%';

```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>female_actors_starting_with_N</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>355</td>
        </tr>
    </tbody>
</table>



Question 43

Which genre has, on average, the lowest movie popularity score? 


**Options:**

 - Science Fiction
 - Animation
 - Documentary
 - Foreign


```sql
%%sql
PRAGMA table_info(genremap);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>movie_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>genre_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>2</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
PRAGMA table_info(genres);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>genre_id</td>
            <td>INTEGER</td>
            <td>1</td>
            <td>None</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>genre_name</td>
            <td>varchar(50)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT
o.year,m.title,ROUND(AVG(m.popularity),0) AS AVG_popularity
FROM oscars AS o
JOIN movies AS m
ON m.movie_id=gm.movie_id
JOIN genremap AS gm
ON gm.movie_id=m.movie_id
JOIN genres AS g 
ON g.genre_id=gm.genre_id
GROUP BY m.title
ORDER BY AVG_popularity ASC LIMIT 10;

```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>year</th>
            <th>title</th>
            <th>AVG_popularity</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1928</td>
            <td>10 Days in a Madhouse</td>
            <td>0.0</td>
        </tr>
        <tr>
            <td>1928</td>
            <td>16 to Life</td>
            <td>0.0</td>
        </tr>
        <tr>
            <td>1928</td>
            <td>1982</td>
            <td>0.0</td>
        </tr>
        <tr>
            <td>1928</td>
            <td>20 Dates</td>
            <td>0.0</td>
        </tr>
        <tr>
            <td>1928</td>
            <td>3 Backyards</td>
            <td>0.0</td>
        </tr>
        <tr>
            <td>1928</td>
            <td>51 Birch Street</td>
            <td>0.0</td>
        </tr>
        <tr>
            <td>1928</td>
            <td>8 Days</td>
            <td>0.0</td>
        </tr>
        <tr>
            <td>1928</td>
            <td>8: The Mormon Proposition</td>
            <td>0.0</td>
        </tr>
        <tr>
            <td>1928</td>
            <td>A Beginner&#x27;s Guide to Snuff</td>
            <td>0.0</td>
        </tr>
        <tr>
            <td>1928</td>
            <td>Alexander&#x27;s Ragtime Band</td>
            <td>0.0</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT g.genre_id,g.genre_name,m.movie_id,gm.movie_id,
ROUND(AVG(m.popularity),0) AS AVG_popularity
FROM movies AS m
JOIN genremap AS gm
ON gm.movie_id=m.movie_id
JOIN genres AS g 
ON g.genre_id=gm.genre_id
GROUP BY g.genre_name
ORDER BY AVG_popularity ASC LIMIT 2

---FOREIGN HAS THE LOWEST AVERAGE POPULARITY OF 1%
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>genre_id</th>
            <th>genre_name</th>
            <th>movie_id</th>
            <th>movie_id_1</th>
            <th>AVG_popularity</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>10769</td>
            <td>Foreign</td>
            <td>7509</td>
            <td>7509</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>99</td>
            <td>Documentary</td>
            <td>291</td>
            <td>291</td>
            <td>4.0</td>
        </tr>
    </tbody>
</table>



Question 44
Which award category has the highest number of actor nominations (actors can be male or female)? (Hint: `Oscars.name` contains both actors' names and film names.)
Options:

- Special Achievement Award
- Actor in a Supporting Role
- Actress in a Supporting Role
- Best Picture




```sql
%%sql 
SELECT * FROM oscars LIMIT 5;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>year</th>
            <th>award</th>
            <th>winner</th>
            <th>name</th>
            <th>film</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1928</td>
            <td>Actor</td>
            <td>None</td>
            <td>Richard Barthelmess</td>
            <td>The Noose</td>
        </tr>
        <tr>
            <td>1928</td>
            <td>Actor</td>
            <td>1.0</td>
            <td>Emil Jannings</td>
            <td>The Last Command</td>
        </tr>
        <tr>
            <td>1928</td>
            <td>Actress</td>
            <td>None</td>
            <td>Louise Dresser</td>
            <td>A Ship Comes In</td>
        </tr>
        <tr>
            <td>1928</td>
            <td>Actress</td>
            <td>1.0</td>
            <td>Janet Gaynor</td>
            <td>7th Heaven</td>
        </tr>
        <tr>
            <td>1928</td>
            <td>Actress</td>
            <td>None</td>
            <td>Gloria Swanson</td>
            <td>Sadie Thompson</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
PRAGMA table_info(oscars);
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>cid</th>
            <th>name</th>
            <th>type</th>
            <th>notnull</th>
            <th>dflt_value</th>
            <th>pk</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>year</td>
            <td>varchar(10)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>1</td>
            <td>award</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2</td>
            <td>winner</td>
            <td>varchar(10)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>3</td>
            <td>name</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
        <tr>
            <td>4</td>
            <td>film</td>
            <td>varchar(500)</td>
            <td>0</td>
            <td>NULL</td>
            <td>0</td>
        </tr>
    </tbody>
</table>



Which award category has the highest number of actor nominations (actors can be male or female)? (Hint: Oscars.name contains both actors' names and film names.)


```sql
%%sql
SELECT award, COUNT(*) AS Actor_nominations
FROM Oscars
WHERE award LIKE '%Actor%' OR award LIKE '%Actress%'
GROUP BY award
ORDER BY Actor_nominations DESC;
```

     * sqlite:///TMDB-a-4006.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>award</th>
            <th>Actor_nominations</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Actress in a Supporting Role</td>
            <td>400</td>
        </tr>
        <tr>
            <td>Actor in a Supporting Role</td>
            <td>400</td>
        </tr>
        <tr>
            <td>Actress</td>
            <td>236</td>
        </tr>
        <tr>
            <td>Actor</td>
            <td>232</td>
        </tr>
        <tr>
            <td>Actress in a Leading Role</td>
            <td>200</td>
        </tr>
        <tr>
            <td>Actor in a Leading Role</td>
            <td>200</td>
        </tr>
    </tbody>
</table>



Question 45

For all of the entries in the Oscars table before 1934, the year is stored differently than in all the subsequent years. For example, the year would be saved as “1932/1933” instead of just “1933” (the second indicated year).  Which of the following options would be the appropriate code to update this column to have the format of the year be consistent throughout the entire table (second indicated year only shown)?
Options:

- `UPDATE Oscars SET year = RIGHT(year, -4)`
- `UPDATE Oscars SET year = SELECT substr(year, -4)`
- `UPDATE Oscars SET year = substr(year, -4)`
- `UPDATE Oscars year =  substr(year, 4)`


```sql
%%sql
UPDATE Oscars SET year = substr(year, -4);
```

     * sqlite:///TMDB-a-4006.db
    9964 rows affected.
    




    []



### Question 46

DStv will be having a special week dedicated to the actor Alan Rickman. Which of the following queries would create a new _view_ that shows the titles, release dates, taglines, and overviews of all movies that Alan Rickman has played in?

Options:

- SELECT title, release_date, tagline, overview 
FROM Movies LEFT JOIN Casts ON Casts.movie_id = Movies.movie_id Left JOIN Actors ON Casts.actor_id = Actors.actor_id 
WHERE Actors.actor_name = 'Alan Rickman'
AS VIEW Alan_Rickman_Movies

- CREATE VIEW Alan_Rickman_Movies AS  
SELECT title, release_date, tagline, overview FROM Movies  
LEFT JOIN Casts ON Casts.movie_id = Movies.movie_id Left JOIN Actors
ON Casts.actor_id = Actors.actor_id
WHERE Actors.actor_name = 'Alan Rickman' 


- CREATE NEW VIEW  Name  = Alan_Rickman_Movies AS SELECT title, release_date, tagline, overview FROM Movies LEFT JOIN Casts ON Casts.movie_id = Movies.movie_id Left JOIN Actors ON Casts.actor_id = Actors.actor_id WHERE Actors.actor_name = 'Alan Rickman'

- VIEW Alan_Rickman_Movies AS SELECT title, release_date, tagline, overview FROM Movies LEFT JOIN Casts ON Casts.movie_id = Movies.movie_id Left JOIN Actors ON Casts.actor_id = Actors.actor_id WHERE Actors.actor_name = 'Alan Rickman'


```sql
%%sql
CREATE VIEW Alan_Rickman_Movies AS
SELECT 
title, 
release_date, 
tagline, 
overview 
FROM Movies
LEFT JOIN 
Casts 
ON 
Casts.movie_id = Movies.movie_id 
Left JOIN Actors 
ON 
Casts.actor_id = Actors.actor_id 
WHERE Actors.actor_name = 'Alan Rickman';
```

     * sqlite:///TMDB-a-4006.db
    (sqlite3.OperationalError) view Alan_Rickman_Movies already exists
    [SQL: CREATE VIEW Alan_Rickman_Movies AS
    SELECT 
    title, 
    release_date, 
    tagline, 
    overview 
    FROM Movies
    LEFT JOIN 
    Casts 
    ON 
    Casts.movie_id = Movies.movie_id 
    Left JOIN Actors 
    ON 
    Casts.actor_id = Actors.actor_id 
    WHERE Actors.actor_name = 'Alan Rickman';]
    (Background on this error at: https://sqlalche.me/e/20/e3q8)
    

### Question 15

Which of the statements about database normalisation are true?

**Statements:**
 
i) Database normalisation improves data redundancy, saves on storage space, and fulfils the requirement of records to be uniquely identified.

ii) Database normalisation supports up to the Third Normal Form and removes all data anomalies.

iii) Database normalisation removes inconsistencies that may cause the analysis of our data to be more complicated.

iv) Database normalisation increases data redundancy, saves on storage space, and fulfils the requirement of records to be uniquely identified.

**Options:**

 - (i) and (ii)
 - (i) and (iii)
 - (ii) and (iv)
 - (iii) and (iv)
