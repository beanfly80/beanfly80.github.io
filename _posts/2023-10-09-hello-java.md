---
layout: post
title:  "Hello Java"
categories: dev
date : 2023-10-09 20:17:43 +09:00
---

- overview
    - Write once, run anywhere -> platform-independent
    - OOP Language
    - Garbage Collection
    - Network Programming using Java API
    - Dynamic Loading at run time -> Performance Risk

- reference
    - docs.oracle.com/en/java/javase/21/docs/api
    - github.com/openjdk/jdk/blob/jdk-21%2B35/src/java.base/share/classes/java/util/HashMap.java
    - en.wikipedia.org/wiki/Hash_function
    - spring.io/
    - mariadb.com, mariadb.com/kb/en/java-connector-using-gradle/
    - code.visualstudio.com/docs/java/java-tutorial


## Hello Java

Run Process
```
- JDK   +- (.java source code ->) javac compiler
|
- JRE
|       +- (.class byte code ->) class loader 
|       +- JVM
|           +- java interpreter + JIT compiler
|           +- runtime system
|
- host system(windows x64, linux...) machine code 
|
- HW
```

### [1] JVM(Java Virtual Machine, 자바 가상 머신)
Java Application을 실행하기 위한 가상의 기계 환경
- 구조
    - GC(garbage collector) : 참조 종료된 리소스 정리
    - class loader : 실행 시 클래스 로드
    - execute engine : byte code 읽어서 실행
    - runtime data area : JVM 실행 시, OS에서 할당받은 메모리
        1. method(static area) : interface, class(static variable, method, member variable) 구성 요소 저장
        1. heap : GC가 관리
        1. stack : local variable
        1. PC(program counter) Register : 스택 영역의 operand 명령어를 임시 저장 후 CPU에 명령을 전달
        1. native method stack area : java 외 코드를 위한 stack

### [2] Build Tools
컴파일(.class), 링크, 패키징(.jar) 과정을 포함
- JAR(Java ARchive) Files
    - Java의 실행 파일
    - 클래스, 리소스 등을 단일 파일로 패키징 -  배포/재사용/관리가 쉬워짐, 서명 추가 가능


- 종류
    1. Make

    1. Apache Ant (2000~)
        - 빌드 파일 : build.xml
        - 절차지향

    1. Apache Maven (2002~)
    자바 소스를 compile하고 package해서 deploy하는 일을 자동화
        - 빌드 파일 : pom.xml
        - pros : 계층적인 데이터 표현, Dependency 관리
        - cons : XML기반이라 가독성 낮음, Ant에 비해 작성하기 어려움

    1. Gradle (2012~)
        - Ant, Maven 기반으로 개발됨
        - pros
            - 빌드 자동화
            - Groovy 기반 DSL(Domain-Specific Language)로 작성하여 가독성이 좋음

### [3] APIs
1. String/ StringBuffer/ StringBuilder Class
    - java.lang.String
        - Represents character strings
        - constant - their values cannot be changed after they are created.
        - Compare creation: 
            ````
            String a1 = new String("java"); // Creates a new string on heap
            String a2 = "java";             // Creates a new string in the String constant pool on heap
            String b = a1;
            String c = a1.toString();
            ````
        - intern()
            ```
            // A pool of strings, initially empty.
            // Returns a canonical representation for the string object.

            public native String intern();
            ```
            - ex.
                ```
                String a = "java";  // located in string pool of heap
                String b = "java";
                String c = new String("java");  // located in heap
                String d = new String("java");
                String e = d.intern();  // shared "java" in spring pool

                System.out.println(a==b); // true
                System.out.println(c==d); // false
                System.out.println(a==c); // false
                System.out.println(a==e); // true
                ```
    - java.lang.StringBuffer
        - A thread-safe, mutable sequence of characters.
        - A string buffer is like a String, but can be modified.
    - java.lang.StringBuilder
        - A mutable sequence of characters.
        - This class provides an API compatible with StringBuffer, but with no guarantee of synchronization.(faster than StringBuffer)

1. HashTable/HashMap Collection
    - Store unique key(in vector) and value(in linked list) pairs in a table. 
        - Key is hashed by Hash function.
        - The resulting hash code is used as the index at which the value is stored within the table.
    - Hash function
        - To map data of arbitrary size to fixed-size values
        - The values returned are called : hash value = hash code = digest = hash
        - EX. MD5, SHA***, CRC32
        - preimage resistance
        - collision resistance
            - Hash collision(= Hash clash) : Pieces of different data in a hash table share the same hash value.
        - Guarantee data integrity & security
    - HashTable : Legacy of HashMap. thread safe.
    - HashMap
        - Allows one null key and multiple null values.
        - not thread-safe - In case of multi-threads, use ConcurrentHashMap or Collections synchronized API
        - To maintain insertion order, use LinkedHashMap, LinkedHashSet.
        - https://stackoverflow.com/questions/10894558/how-hashtable-and-hashmap-key-value-are-stored-in-the-memory
    - Layer
        ```
        Map(Interface)
            +- AbstractMap 
                +- MashMap (use ArrayList) 
                    +- LinkedHashMap (use LinkedList)
        ```
    - 왜 hashmap 검색이 빠른가
        1. hased key가 index되어 이용됨
        1. shift operation 이용하여, CPU 사용량 줄임 

        ```
        //https://github.com/openjdk/jdk/blob/jdk-21%2B35/src/java.base/share/classes/java/util/HashMap.java
        
        public final int hashCode() {
            return Objects.hashCode(key) ^ Objects.hashCode(value);
        }

        public V put(K key, V value) {
            return putVal(hash(key), key, value, false, true);
        }
        final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                    boolean evict) {
            Node<K,V>[] tab; Node<K,V> p; int n, i;
            if ((tab = table) == null || (n = tab.length) == 0)
                n = (tab = resize()).length;
            if ((p = tab[i = (n - 1) & hash]) == null)
                tab[i] = newNode(hash, key, value, null);
            //// ...skip
        }

        public V get(Object key) {
            Node<K,V> e;
            return (e = getNode(key)) == null ? null : e.value;
        }
        final Node<K,V> getNode(Object key) {
            Node<K,V>[] tab; Node<K,V> first, e; int n, hash; K k;
            if ((tab = table) != null && (n = tab.length) > 0 &&
                (first = tab[(n - 1) & (hash = hash(key))]) != null) {
                if (first.hash == hash && // always check first node
                    ((k = first.key) == key || (key != null && key.equals(k))))
                    return first;
                if ((e = first.next) != null) {
        //Skip
        ```

## Spring Framework
Infrastructural support at Java application level
-  Modules : ref. https://docs.spring.io/

![modules](https://docs.spring.io/spring-framework/docs/5.0.0.RC2/spring-framework-reference/images/spring-overview.png)

- Features
    - Core technologies: dependency injection, events, resources, i18n, validation, data binding, type conversion, SpEL, AOP.
    - Testing: mock objects, TestContext framework, Spring MVC Test, WebTestClient.
    - Data Access: transactions, DAO support, JDBC, ORM, Marshalling XML.
    - Spring MVC and Spring WebFlux web frameworks.

- org.springframework.web.servlet.mvc

    WS(Web Server) & WAS(Web Application Server)
    
    <img src="/assets/hello_java_ws_was.png" width="500px"/>
    
    - WS : Legacy. ex)apache, nginx
    - WAS : ex)tomcat
    - Web Container : read web.xml -> create a thread -> request Servlet

### Tips
- How to debug bootRun with VScode
    - Edit .vscode/settings.json
    ```
    {
        "gradle.javaDebug": {
            "tasks": [
                "bootRun"
            ],
            "clean": true
        }
    }
    ```
    - In gradle, next to Run Task will apear the Debug Task option.

- Web server failed to start. Port XXXX was already in use.
    ```
    # Linux Shell
    netstat -ano | grep XXXX
    kill _PID_
    
    # Windows CMD
    netstat -ano | findstr XXXX
    taskkill /pid _PID_ /f
    ```

- [Bit Shift Operators](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op3.html)
    - signed left shift operator "<<" shifts a bit pattern to the left
    - signed right shift operator ">>" shifts a bit pattern to the right. The bit pattern is given by the left-hand operand, and the number of positions to shift by the right-hand operand.
    - unsigned right shift operator ">>>" shifts a zero into the leftmost position, while the leftmost position after ">>" depends on sign extension.

