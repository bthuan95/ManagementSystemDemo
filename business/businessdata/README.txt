################     Create DB    ######################### 

create table user (
   ID BIGINT NOT NULL AUTO_INCREMENT,
   USERNAME VARCHAR(30) NOT NULL,
   PASSWORD VARCHAR(100) NOT NULL,
   FIRSTNAME VARCHAR(30) NOT NULL,
   LASTNAME  VARCHAR(30) NOT NULL,
   EMAIL VARCHAR(30) NOT NULL,
   PRIMARY KEY (ID),
   UNIQUE (USERNAME)
);
   
/* ROLE table contains all possible roles */ 
create table role(
   ID BIGINT NOT NULL AUTO_INCREMENT,
   ROLE_NAME VARCHAR(30) NOT NULL,
   PRIMARY KEY (ID),
   UNIQUE (ROLE_NAME)
);
   
/* JOIN TABLE for MANY-TO-MANY relationship*/  
CREATE TABLE user_role (
    USER_ID BIGINT NOT NULL,
    ROLE_ID BIGINT NOT NULL,
    PRIMARY KEY (USER_ID, ROLE_ID),
    CONSTRAINT FK_USER FOREIGN KEY (USER_ID) REFERENCES user (ID),
    CONSTRAINT FK_ROLE FOREIGN KEY (ROLE_ID) REFERENCES role (ID)
);
  
################     INSERT DATA    ######################### 
INSERT INTO role(ROLE_NAME)
VALUES ('USER');
  
INSERT INTO role(ROLE_NAME)
VALUES ('ADMIN');
  
  
/* usr: admin / password: ‘abc125’ */
INSERT INTO user(USERNAME, PASSWORD, FIRSTNAME, LASTNAME, EMAIL)
VALUES ('admin','$2a$10$4eqIF5s/ewJwHK1p8lqlFOEm2QIA0S8g6./Lok.pQxqcxaBZYChRm', 'Sam','Smith','samy@xyz.com');
  
  
/* Populate JOIN Table */
INSERT INTO user_role (USER_ID, ROLE_ID)
  SELECT USER_ID, ROLE_ID FROM user user, role role
  where user.USERNAME='admin' and role.ROLE_NAME='ADMIN';
 
/* Create persistent_logins Table used to store rememberme related stuff*/
CREATE TABLE persistent_logins (
    USER_NAME VARCHAR(64) NOT NULL,
    SERIES VARCHAR(64) NOT NULL,
    TOKEN VARCHAR(64) NOT NULL,
    LAST_USED TIMESTAMP NOT NULL,
    PRIMARY KEY (SERIES)
);