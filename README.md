# exploration

exploration for q4 / java api | server

out-line

- quick overview of the basics of java the language, what it is ,and why we use.
- Setting up IDE with needed dependancies.
- How to use spark framework to create RESTful routes
- accessing that database using springs framework JDBC
- Sending the information to the client.

rough draft

**Basics of Java** _Java is a strictly typed language, this simply means that
it variables have to be declared at creation and the developer does not have
the ability to change the data type after the variable is created. You also must
declare if the value will be a string "String", integer "int", decimal "float",
character "char" extra.
__
```java
   String someString = "hello this is some string";
   char firstLetter = "T";
   int number = 5;
   float otherNumber = 5.663;
   int[5] myArray = new int[];
   myArray[0] = 5;
   myArray[4] = 3;
   int[] newArray = {1,2,3,4,5,6};
```

_Java is not a scripting language like Javascript or python, but rather it is
strictly oop. Everything in java is a class, meaning even if you want to create
the simplest lines of code you must create a class.

_Every Java program has a main function. This function begins the whole
program. Nothing happens until the main function is invoked. This is where you
include the code you need to get the program off the floor and begin running.
__
```java
    public class Hello{
        public Hello(param1, param2){
            this.param1 = param1;
            this.param2 = param2;
        }//constructor function
        public static void main(String[] args) {
            Hello newHello = new Hello(param1, param2);//creating instance of Hello
        }//main function
    }
```

_In Java we store each class in a different file, this is for organization  and
separation of concerns. This may not seem necessary, however once your project
begins to grow you would soon find complexity issues with your project

_As the great Tyler Bettilyon told me there are four steps to learning a new
language. Step one creating a Hello World! function, step two iterate through
an Array, step three a simple recursion function, and the fourth step create a
complex algorithm. If you can do the above four you are ready!
__

```java
class HelloWorldApp {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

**setting up IDE and environment**

_With Java I recommened not using a text editor we must use a IDE. With an IDE
we will able run and debug our code in the application instead of the
terminal/console. You can either download eclipse or intellij, for this
tutorial we are going to use intellij.

_Next step before we jump into our new download IDE is to download a few
dependancies for our project. Dependancies are libraries that are needed by your
program.

_The first dependency we need is maven. Maven is a package manager for Java,
equivalint to npm for javascript. In you IDE you will be able to create a
project with Maven. Maven will ask you a serious of questions which will be
used create the basics of your project, file system, name, and pom.

_To create a new maven project in intellij go to file -> new -> project.
A pop-up window will appear and choose to create a project with maven on the
left menu bar.

_the pom.xml file is where are your dependancies will be held. We will continue
to make changes to this file throughout the tutorial.

_We will also use the key word import. This key word is used to allow the user
to have access to special functionality that is not normally shipped with java.
the nice thing about using an IDE is they will detect the usage of specific
imports and add them for you. Below are the import we will need for this
tutorial.
__

```java
    import com.fasterxml.jackson.databind.ObjectMapper;
    import java.io.IOException;
    import java.io.StringWriter;
    import java.sql.*;
    import java.util.HashMap;
    import java.util.Map;
    import static spark.Spark.*;
    import static spark.Spark.port;
```

**How to use spark framework to create RESTful routes**

_For our API we will have to create RESTful routes, each route will send a
different body of data depending on the users request. To create these routes
we will use the java framework spark. For this API and most API's in general we
will only need to use the get verb. Reason being, we do not the user to be able
to edit our database in any way.

_Time to make our first edit to the pom.xml file that was create by maven. You
should easily be able to find the dependency by doing a quick google search.
The dependency you find should look very similar to the one below, however you
may be using a different version.

_You will also have to add the SLF4J dependency, this dependency enables
logging for debugging which spark needs.
__

```xml
<dependancies>
  <dependency>
      <groupId>com.sparkjava</groupId>
      <artifactId>spark-core</artifactId>
      <version>2.5</version>
  </dependency>
  <dependency>
     <groupId>org.slf4j</groupId>
     <artifactId>slf4j-jdk14</artifactId>
     <version>1.7.22</version>
     <scope>compile</scope>
   </dependency>
 </dependencies>
```

_After we add the dependencies above correctly it is time add the port our
localhost server will be listing on. This will be a simple line of code.
__

```java
    port(8000); //port number
```
_Now when you run your code your localhost will be listing on the port you
stated. Now it is time to create a RESTful, I will leave an example of a get
route take a look.
__

```java
    get("/hello", (request, response) -> {
        //your code here
        })
```
_Lets walk through this code. Get is the verb we are using hello is the path
that when visited will feed the data. Request and response are parameters that
can be used to get and send data to the user. Inside the curly's we will enter
the code need.
__

**Connecting to your postgres database using JDBC**

_First we will have to add another dependency to our pom.xml file. Depending on
the database being used you are going to use is directly going to reflect on
the dependency you need to add to your pom.xml file. Below is the dependency a
postgres database.
__

```xml
<dependency>
      <groupId>org.postgresql</groupId>
      <artifactId>postgresql</artifactId>
      <version>9.4.1212</version>
  </dependency>
```

_We need to now load the JDBC driver for postgres this is what allows our java
application to interact with the database.
__

```java
   Class.forName("org.postgresql.Driver");
```
 _it is best practice to but the code above in a try catch block incase it does
 not work successfully you are able to through the proper error.
__

```java
    try {
       Class.forName("org.postgresql.Driver");
    } catch (ClassNotFoundException e) {
       System.out.println("Class not found " + e);
    }
```

_In JDBC the database is represented by a URL when using postgres it takes
the following forms depending on how you have set up your database.
__
```java
    jdbc:postgresql:database
    jdbc:postgresql://host/database
    jdbc:postgresql://host:port/database;

```

_There are a total of three connection parameters the url is the first the
second and third are the username password for the database in use. We will
create three variables represent all the parameters.
__
```java
    String URL = "jdbc:postgresql://localhost/database";
    String USER = "name";
    String PASS = "password";
```

_Now we will create the connection to our database and store it in variable of
your choice.
__

```java
    Connection conn = DriverManager.getConnection(URL, USER, PASS);
```

_We will us this variable to create another called st short for Statement. With
this variable we will be able to create queries.
__

```java
    Statement st = conn.createStatement();
```

_Now we can create queries to our database using the executeQuery function.
This function take string, which is the query search you want to run against the
database you are working.
__
```java
    ResultSet rs = st.executeQuery("query string");
```

_Now we can search through the data and grab what wish using the following
while loop.
__

```java
    while (rs.next()) {
        System.out.println(rs.getString(1)); //print to the console the data found in the first column position for every row.
    }
    rs.close(); //closes the ResultSet because you can only ever have one at a time
    st.close(); //closes the statement so you can create another request.
```

_There a lot of things that can be done inside the while loop to get the data
you want but however it become tricky the more information you want. Below is
the code I used to grab an entire table from my database.
__

```java
    Map<String, Map<String, String>> books = new HashMap<>();
    ResultSetMetaData rsmd = rs.getMetaData();
    while (rs.next()) {
        int numColumns = rsmd.getColumnCount();
        HashMap<String, String> inner = new HashMap<>();
        for(int i = 1; i <= numColumns; i++) {
            inner.put(rsmd.getColumnName(i), rs.getString(i));
            }
        books.put(rs.getString(1), inner);
    }
    rs.close();
    st.close();
```

_The above code is creating a map object inside another map object. It create a
new instance of an inner map object for each iteration of the while loop and
pushes the instance to the books map after each iteration creating a JSON like
object.
__

**Sending the information to the client**

_Now that we have gotten the information we wanted from our database and hvae
hopefully saved in a variable it is time turn it into JSON and send it to the
client using one of out routes that we have created. First we need to add
another dependency called Jackson. Jackson the standard JSON library for JAVA.
It will allow you to parse and generate JSON.
__

```xml
    <dependency>
       <groupId>com.fasterxml.jackson.core</groupId>
       <artifactId>jackson-databind</artifactId>
       <version>2.5.3</version>
    </dependency>
```

_Now it is time to create your routes if you haven't already and give them a
path makes sense to you and what kind of information you will be serving the
client.
__
```java
    get("/books", (request,response) ->{
           StringWriter stringWriter = new StringWriter();
           objectMapper.writeValue(stringWriter, map);
           response.type("application/json");
           response.header("Access-Control-Allow-Origin", "*");
           return stringWriter;
     });
```

_Lets walk through through this code so we can better understand what is
happening. Any one who makes a get request to my domain/books will hit my api
which will send back JSON. We first create a StringWriter, at its simplest
state it works similar to .toString(). ObjectMapper is part of Jackson that has
many methods you can use with it in this case we want to convert our object to
JSON so we will use writeValue.writeValue first param is what the second param
our object will be written to.

_We then set the response type to JSON and then we need to make changes to the
header because of CORS or cross origin resource sharing. CORS covers the web
application architecture where a given web application calls api resources
located in a different domain other than the domain of the web application.
What this means if a web application is making request to your api from domain
unless you set specific headers in the request and response domain call there
will be an error.
__

**You Are Done**
