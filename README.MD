== Spring Boot Liquibase Sample

This sample demonstrates the liquibase auto-configuration support.

You can look at `http://localhost:8080/actuator/liquibase` to review the list of
scripts.

This sample also enables the H2 console :
1. set application.properties: spring.profiles.active=h2
2. run app
3. http://localhost:8080/h2-console
4. set jdbc url is `jdbc:h2:mem:testdb`.
5. set user = sa, password=sa
6. review database:

    SELECT * FROM PERSON;
    SELECT * FROM user;
    SELECT * FROM DATABASECHANGELOG ;
    SELECT * FROM DATABASECHANGELOGLOCK ;

7. set application.properties: spring.profiles.active=mysql
8. run app
9. connect to db
10. set user = sa, password=sa
11. review  database

12. set application.properties: spring.profiles.active=mysql2
13. change db.changelog-master.yaml to work with 1st file only
14. run app
15. connect to db
16. set user = sa, password=sa
17. review  database without view

18. download liquibase from http://download.liquibase.org/download
18. make diff file between 2 schemas: testdb, testdb2
19. rollback : 
     liquibase  --defaultsFile=liquibase.properties rollback 1.0
20. make diff again


liquibase \
        --classpath=/Users/samuelc/.m2/repository/mysql/mysql-connector-java/5.1.46/mysql-connector-java-5.1.46.jar \
        --driver=com.mysql.jdbc.Driver \
        --url=jdbc:mysql://localhost:3306/testdb?useUnicode=yes&characterEncoding=UTF-8&autoReconnect=true&useSSL=false \
        --username=sa \
        --password=sa \
    diff \
        --referenceUrl=jdbc:mysql://localhost:3306/testdb2?useUnicode=yes&characterEncoding=UTF-8&autoReconnect=true&useSSL=false \
        --referenceUsername=sa \
        --referencePassword=sa

21. make doc file on db

liquibase \
        --classpath=/Users/samuelc/.m2/repository/mysql/mysql-connector-java/5.1.46/mysql-connector-java-5.1.46.jar \  
        --driver=com.mysql.jdbc.Driver \
        --url=jdbc:mysql://localhost:3306/testdb?useUnicode=yes&characterEncoding=UTF-8&autoReconnect=true&useSSL=false \
        --username=sa \
        --password=sa \
        --changeLogFile=path/to/changelog.xml \
    dbDoc \
        /docs/dbdoc
        
note: sometimes if the update application is abruptly stopped, then the lock remains stuck.

Then running

UPDATE DATABASECHANGELOGLOCK SET LOCKED=FALSE, LOCKGRANTED=null, LOCKEDBY=null where ID=1;