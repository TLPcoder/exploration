# exploration

**overview of tutorial**

_This tutorial assumes you have basic programing knowledge and know how to setup a database. The first part will go over some of the basics of Java. Declaring variables, creating a hello world program, and the philosophies of Java as a program. The second part I will go over how to create an API in Java. I will go through all you need from importing the needed libraries and framework to connecting to a database. Lets get started.__

**Basics of Java**

_Java is a strictly typed language. This simply means variable type must be
declared at creation. The value can be a string "String", integer "int",
decimal "float", character "char", and more. You do not have the ability to
change the data type after the variable is created. Below are some examples. __

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

_Java is not a scripting language like Javascript or Python, but rather it is strictly OOP. Everything in java is a class, meaning even for the simplest lines of code a class must be created.

_Every Java program has a main function, which begins the whole program.
Nothing happens until the main function is invoked. This is where you include
the code you need to get the program off the floor and begin running. __

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

_In Java you store each class in a different file or folder. This is for
organization and separation of concerns. This step may not seem necessary, once
your project begins to grow, you would soon find complexity issues with your
project. We connect these files using the key word "package". A package is a
namespace that organizes a set of related classes. Conceptually
you can think of packages as being similar to different folders on your
computer. Because software written in the Java programming language can be composed of hundreds or thousands of individual classes, it makes sense to keep things organized by placing related classes into packages.

_There should always be four steps taken when learning a new language. Step
one: creating a Hello World! function. Step two: iterate through an Array. Step
three: a simple recursion function. Step four: create a complex algorithm. If
you can do the above four you are ready! __

Hello World
```java
class HelloWorldApp {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```
_The word void is used in method declaration to state that the method returns
nothing. This brings me to the topic of method declaration. Each method has to
be declaration with the data type it will return and as stated above if the
function return nothing it is declared with the word void.__

Iterate through Array

```java
public class Iterate_Array{
    static int[] myArray = {1,2,3,345,34,6254,656,538};
    Iterate_Array(){

    }
    public void goThrowArray(int[] arr){
        for(int i = 0; i < arr.length; i++){//for loop iterating through myArray
            System.out.println(arr[i]);//logs to the console every element in the array
        }
    }
    public static void main(String[] args) {
        Iterate_Array iterate = new Iterate_Array();//creating instance of
        Iterate_Array
        iterate.goThrowArray(myArray);//calling method goThrowArray
    }
}
```
_The word static in Java tells the program that the variable declared belongs
to the class not any particular instance of the class but the class itself.
Meaning every instance of the class has access to the variable, and can make
changes to the variable.__

Recursion

```java
public class Fibonacci{
    Fibonacci(){

    }

    public int run(int num){
        if (num < 2) {
            return 1;
        }else{
            return run(num - 1) + run(num - 2);
        }
    }

    public static void main(String[] args) {
        Fibonacci fib = new Fibonacci();
        System.out.println(fib.run(5));
    }
}
```
Binary Search
```java

public class BinarySearch{
    static int[] values = {1,2,3,5,6,7,9};
    BinarySearch(){

    }
    public static void main(String[] args) {
        BinarySearch search = new BinarySearch();
        System.out.println("answer " + (search.searchArray(values, 0,
        values.length - 1, 3)));
        }
    public int searchArray(int[] arr, int left, int right, int value){
        int middle = (left + right) / 2;
        if(arr[middle] == value){
            return middle;
        }
        if(middle < 0 || middle >= arr.length - 1){
            return -1;
        }else if(arr[middle] < value){
            return searchArray(arr, middle + 1, right, value);
        }else{
            return searchArray(arr, 0, middle - 1, value);
        }
    }
}

```
**setting up IDE and environment**

_With Java we are going to use an IDE. With an IDE you will able run and debug
our code in the application instead of the terminal/console. You can either
download Eclipse or Intellij. For this tutorial you are going to use Intellij.

download Intellij https://www.jetbrains.com/idea/download/

_Next step before we jump into our new downloaded IDE. One needs to download a
few dependancies for our project. Dependancies are libraries that are needed by
your program.

_The first dependency we need is Maven. Maven is a package manager for Java,
equivalent to npm for JavaScript. In your IDE you will be able to create a
project with Maven. Maven will ask you a series of questions which will be used
create the basics of your project: file system, name, and pom. A pom file is
similar to your package.json for node. It stores all the dependancies for your
application.

download maven https://maven.apache.org/download.cgi;

_To create a new Maven project in Intellij, go to file -> new -> project. A
pop-up window will appear and choose to create a project with Maven on the left
menu bar.

_We will also use the key word import. This key word allow's the user to have
access to special functionality that is not normally shipped with Java. The
nice thing about using an IDE is they will detect the usage of specific imports
and add them for you. Below are the import we will need for this tutorial. __

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

_I am not going to go into depth on what each import is doing but if you are
curious I recommend looking into.__

**How to use spark framework to create RESTful routes**

_For our API we will have to create RESTful routes. Each route will send a
different body of data depending on the users request. To create these routes
we will use the Java framework Spark. For this API and most APIs in general, we
will only need to use the GET verb. As we do not the user to be able
to edit our database in any way.

_Time to make our first edit to the pom.xml file that was created by Maven. You should easily be able to find the dependency by doing a quick google search. The dependency you find should look very similar to the one below. (you may be using a different version)

_You will also have to add the SLF4J dependency, this dependency enables logging for debugging which spark needs. __

```xml
<dependancies>
  <dependency>
      <groupId>com.sparkjava</groupId>  <!-- this is our route framework dependency-->
      <artifactId>spark-core</artifactId>
      <version>2.5</version>
  </dependency>
  <dependency>
     <groupId>org.slf4j</groupId> <!-- this is a dependency for spark-->
     <artifactId>slf4j-jdk14</artifactId>
     <version>1.7.22</version>
     <scope>compile</scope>
   </dependency>
 </dependencies>
```

_After we add the dependencies above, let's add the port our localhost server
will be listening on. This takes a simple line of code. __

```java
    port(8000); //port number
```

_Now when you run your code your localhost will be listing on the port you
stated. Now it is time to create a RESTful. Take a look, the GET route sample
below.__

```java
    get("/hello", (request, response) -> {
        //your code here
        })
```

_Let's walk through this code. GET is the verb we are using. Hello is the path
that, when visited, will provide the data. Request and response are parameters
that can be used to retrieve and send data to the user. Inside the braces, we
will enter the code needed. __

**Connecting to your postgres database using JDBC**

_First we will have to add another dependency to our pom.xml file. Depending on the database being used you are going to use is directly going to reflect on the dependency you need to add to your pom.xml file. Below is the dependency a postgres database. __

```xml
<dependency>
      <groupId>org.postgresql</groupId>
      <artifactId>postgresql</artifactId>
      <version>9.4.1212</version>
  </dependency>
```

_We need to now load the JDBC driver for postgres. This is what allows our Java
application to make request to your database. JDBC is to Java what knex.js is
to JavaScript.__

```java
   Class.forName("org.postgresql.Driver");
```

_It is best practice to put the code above in a try catch block incase it does not work successfully then you will be able to throw the proper error. __

```java
    try {
       Class.forName("org.postgresql.Driver");
    } catch (ClassNotFoundException e) {
       System.out.println("Class not found " + e);
    }
```

_In JDBC the database is represented by a URL when using postgres. It may take the following forms depending on how you have set up your database. __

```java
    jdbc:postgresql:database
    jdbc:postgresql://host/database
    jdbc:postgresql://host:port/database;
```

_There are a total of three connection parameters: the url is the first, while the second and third are the username and password for the database in use. We will create three variables to represent all the parameters. __

```java
    String URL = "jdbc:postgresql://localhost/database";
    String USER = "name";
    String PASS = "password";
```

_Now we will create the connection to our database and store it in variable of your choice. __

```java
    Connection conn = DriverManager.getConnection(URL, USER, PASS);
```

_We will us this variable to create another called st short for Statement. With this variable we will be able to create queries. __

```java
    Statement st = conn.createStatement();
```

_Now we can create queries to our database using the executeQuery function.
This function takes a string, which is the query search you want to run against the database you are working. __

```java
    ResultSet rs = st.executeQuery("query string");
```

_Now we can search through the data and grab what you wish using the following while loop. __

```java
    while (rs.next()) {
        System.out.println(rs.getString(1)); //print to the console the data found in the first column position for every row.
    }
    rs.close(); //closes the ResultSet because you can only ever have one at a time
    st.close(); //closes the statement so you can create another request.
```

_There a lot of things that can be done inside the while loop to get the data you want but however it become tricky the more information you want. Below is the code I used to grab an entire table from my database. __

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

_The above code creates a map object inside another map object. It creates a
new instance of an inner map object for each iteration of the while loop and
pushes the instance to the books map after each iteration creating a JSON like
object. __

**Sending the information to the client**

_Now that we have gotten the information we wanted from our database and hvae hopefully saved in a variable it is time turn it into JSON and send it to the client using one of out routes that we have created. First we need to add another dependency called Jackson. Jackson the standard JSON library for JAVA. It will allow you to parse and generate JSON. __

```xml
    <dependency>
       <groupId>com.fasterxml.jackson.core</groupId>
       <artifactId>jackson-databind</artifactId>
       <version>2.5.3</version>
    </dependency>
```

_Now it is time to create your routes if you haven't already and give them a path makes sense to you and what kind of information you will be serving the client. __

```java
    get("/books", (request,response) ->{
           StringWriter stringWriter = new StringWriter();
           objectMapper.writeValue(stringWriter, map);
           response.type("application/json");
           response.header("Access-Control-Allow-Origin", "*");
           return stringWriter;
     });
```

_Lets walk through through this code so we can better understand what is happening. Any one who makes a get request to my domain/books will hit my api which will send back JSON. We first create a StringWriter, at its simplest state it works similar to .toString(). ObjectMapper is part of Jackson that has many methods you can use with it in this case we want to convert our object to JSON so we will use writeValue.writeValue first param is what the second param our object will be written to.

_We then set the response type to JSON and then we need to make changes to the header because of CORS or cross origin resource sharing. CORS covers the web application architecture where a given web application calls api resources located in a different domain other than the domain of the web application. What this means if a web application is making request to your api from domain unless you set specific headers in the request and response domain call there will be an error. __

**You Are Done**

code can be found below

```java
import com.fasterxml.jackson.databind.ObjectMapper;

import java.io.IOException;
import java.io.StringWriter;
import java.sql.*;
import java.util.HashMap;
import java.util.Map;

import static spark.Spark.*;
import static spark.Spark.port;

public class Workout {

    private static ObjectMapper objectMapper = new ObjectMapper();

    public static void main(String[] args) throws SQLException, IOException {
        port(8001);
        Map<String, Map<String, String>> books = new HashMap<>();

        try {
            Class.forName("org.postgresql.Driver");
        } catch (ClassNotFoundException e) {
            System.out.println("Class not found " + e);
        }
        try {
            String URL = "jdbc:postgresql://localhost/bookshelf_test";
            String USER = "TrevorP";
            String PASS = "";
            Connection conn = DriverManager.getConnection(URL, USER, PASS);
            Statement st = conn.createStatement();
            ResultSet rs = st.executeQuery("Select * from books");
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

        }
        catch(SQLException es){
            es.printStackTrace();
        }

        get("/hello", (request, response) -> "Hello bro!");


        get("/books", (request,response) ->{
            StringWriter stringWriter = new StringWriter();
            objectMapper.writeValue(stringWriter, books);
            response.type("application/json");
            response.header("Access-Control-Allow-Origin", "*");
            return stringWriter.toString();
        });

        get("/:id", (request, response) -> {
            StringWriter currentBook = new StringWriter();
            objectMapper.writeValue(currentBook, books.get(request.params(":id")));
            response.type("application/json");
            response.header("Access-Control-Allow-Origin", "*");
            return currentBook;
        });

        get("/", (request, response) -> "Looking for books bro?");
    }
}

```
