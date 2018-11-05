#About the logs analysis project:
    Through this project we can draw important data from the database such as how many views of the article and famous authors and errors that occurred during the day, the output will be a plain text file.

##Source of Database:
    you can download it from [Here](https://d17h27t6h515a5.cloudfront.net/topher/2016/August/57b5f748_newsdata/newsdata.zip).

##Install:
    we use PostgreSQL to install it u can type in your terminal 
    ```
    $ pip install psycopg2
    ```


##How to use:
    ```
    import psycopg2
    db = psycopg2.connect(database='databaseName')
    c = db.cursor()
    c.execute(query)
    data = c.fetchall()
    ```

    to write in file ->
        ```
        with open('output.txt', 'w') as out:
            out.write("Type some thing here")
        ```



##Example:
    this function return list of tuple of all colums data from toparticles table . 
    ```
    def most_popular_articles():
        query = "select * from toparticles limit 3;"
        c.execute(query)
        data = c.fetchall()
        return data
    ```

##Creat views:
    1) ```
    create view toparticles as select articles.title, count(*) as view from articles, log where path like '%' || slug || '%' and status = '200 OK' group by articles.title order by view desc;
    ```
    
    2) ```
    create view popular as select A.title,authors.name as author,view from (select toparticles.title, articles.author, view from toparticles ,articles where toparticles.title = articles.title order by view desc) as A, authors where A.author = authors.id;
    ```

    3) ```
    create view error as select (cast(count(status) as decimal)/(select count(method) as method from log))*100 as error,date_trunc('day', time) as date from log group by date_trunc('day', time) order by error desc;
    ```


##Run Program:
    to run the program just click on newsdb.py then file output.txt will be created and contain the information about most three articles and most popular authors and error that occurred during the day.
