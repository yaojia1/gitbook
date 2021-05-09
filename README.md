---
description: .NET project desvription
---

# README

## 1.Architecture

#### Three layer **framework, consist of** four projects

![](.gitbook/assets/v01xsaz82-aucw0e8hg4qnl.png)

### **1\)Model layer: save all the entities**

![archieve](.gitbook/assets/m2g83-ie-8-440wgusd0u87.png)

* Classes and Users are models, which are used to migrate database
* UserInfo and Classes used in all the layers as objects

![](.gitbook/assets/k-7-la-oyilf-4gs-_-b.png)

### **2\)DataAccessLayer**

![](.gitbook/assets/o7v4pytadws5-xv-sv-s-lp.png)

**DBbase:** create and check the connection to the sqlite database

**HebutContext:** config the Creation of the Database from the entity

**Useraccess and Classaccess**: manage the data of users and classes in the database

### **3\)BusinessLayer**

![](.gitbook/assets/r711usd10atusdsfejn1m98vfr6.png)

**manage the students and classes through the DAL**

### **4\)HebutUI**

using the MVC archieve

![Model-Controller-view](.gitbook/assets/tplzd-oul-uwrl-rttj-8a.png)

**Create Controllers by models**

## 2.Reference

UI layer: Business layer, Model

Business layer: DAL, Model

DAL: Model

## 3.Function

### permission:

In Business layer, through the loginAccess class, the user will first login or become a user and the account type will be saved in the database, and when he/she manage the students and classes, the system will first check the permission.

![](.gitbook/assets/nxusdm-bkvdhw3w-tc-1g-f.png)

### the pages

![welcome page](.gitbook/assets/5frg5qdusdkubqriil-0-z-usdo.png)

![login and regist page](.gitbook/assets/09egiusd-r939k-is-ek16usdqq.png)

![user list](.gitbook/assets/8t-zn-u0s-lhjt-c-f-a.png)

![edit user and class](.gitbook/assets/4wm1i-_-qny6fsseu-2pq-w.png)

![create class](.gitbook/assets/vc-e25-45qrl1s_t-mc9y-r.png)

## 

![class page](.gitbook/assets/8oaky-0-z01plrhm8-d-h.png)

## 

