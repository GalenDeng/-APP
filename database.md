#  database数据库 （2017.09.13）

1. `ORM框架Sequelize`
Node的ORM框架Sequelize来操作数据库。这样，我们读写的都是JavaScript对象，Sequelize帮我们把对象变成数据库中的行
Sequelize返回的对象是Promise
ORM技术：Object-Relational Mapping，把关系数据库的表结构映射到对象上
2. `let seqz = app.locals.sequelize as Sequelize`
```
把app.locals.sequelize定义为 Sequelize的值，然后赋值给seqz
```