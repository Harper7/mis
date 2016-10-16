# mis
##管理信息系统
SET FOREIGN_KEY_CHECKS=0;

CREATE TABLE `保养信息` (
`保养ID` int(20) NOT NULL,
`保养类别` varchar(50) NOT NULL,
`保养人` varchar(255) NOT NULL,
`班组` varchar(255) NOT NULL,
PRIMARY KEY (`保养ID`),
KEY `保养类别` (`保养类别`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
INSERT INTO `保养信息` VALUES (1,'月检','李易峰','1组');
CREATE TABLE `保养记录` (
`保养记录ID` int(20) NOT NULL AUTO_INCREMENT,
`设备ID` int(20) NOT NULL,
`保养ID` int(20) NOT NULL,
`保养时间` date NOT NULL,
`说明` varchar(255) DEFAULT NULL,
`消耗ID` int(20) DEFAULT NULL,
PRIMARY KEY (`保养记录ID`),
KEY `设备ID` (`设备ID`),
KEY `保养ID` (`保养ID`),
KEY `消耗ID` (`消耗ID`),
CONSTRAINT `保养ID` FOREIGN KEY (`保养ID`) REFERENCES `保养信息` (`保养ID`),
CONSTRAINT `消耗ID` FOREIGN KEY (`消耗ID`) REFERENCES `消耗记录` (`消耗ID`),
CONSTRAINT `设备ID` FOREIGN KEY (`设备ID`) REFERENCES `设备信息` (`设备ID`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;
INSERT INTO `保养记录` VALUES (1,123456,1,'2016-09-25',null,1);

CREATE TABLE `检修情况` (
`ID` int(20) NOT NULL AUTO_INCREMENT,
`保养内容` varchar(255) NOT NULL,
`完成状况` varchar(255) NOT NULL,
`备注` varchar(255) DEFAULT NULL,
`检修设备ID` int(20) NOT NULL,
PRIMARY KEY (`ID`),
KEY `检修设备ID` (`检修设备ID`),
CONSTRAINT `检修设备ID` FOREIGN KEY (`检修设备ID`) REFERENCES `设备信息` (`设备ID`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
INSERT INTO `检修情况` VALUES (1,'检查接线紧固情况和电缆磨损情况','完成',NULL,123456),(2,'检查液位仪传感器周围漏液和磨损情况','完成','更换传感器',123456),(3,'校对液位仪准确度','完成',NULL,123456);
CREATE TABLE `检修项目` (
`设备类别` varchar(50) NOT NULL,
`保养类别` varchar(50) NOT NULL,
`保养内容` varchar(255) NOT NULL,
KEY `保养类别` (`保养类别`),
KEY `设备类别` (`设备类别`),
CONSTRAINT `保养类别` FOREIGN KEY (`保养类别`) REFERENCES `保养信息` (`保养类别`),
CONSTRAINT `设备类别` FOREIGN KEY (`设备类别`) REFERENCES `设备信息` (`设备类别`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
INSERT INTO `检修项目` VALUES ('采样机','月检','检查电缆电缆头固定情况'),('液位仪','月检','检查各部位限位开关是否灵活'),('液位仪','月检','检查电机接线盒密封情况');


CREATE TABLE `设备信息` (
`设备ID` int(20) NOT NULL,
`设备类别` varchar(50) NOT NULL,
`设备寿命（年）` varchar(50) NOT NULL,
PRIMARY KEY (`设备ID`),
KEY `设备类型` (`设备类别`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
INSERT INTO `设备信息` VALUES (123456,'采样机','8');


CREATE TABLE `消耗记录` (
`消耗ID` int(20) NOT NULL,
`修理内容` varchar(255) NOT NULL,
`消耗材料` varchar(255) NOT NULL,
`消耗数量` int(20) NOT NULL,
`剩余数量` int(20) NOT NULL,
KEY `消耗ID` (`消耗ID`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
INSERT INTO `消耗记录` VALUES (1,'更换启停按钮','按钮',1,19);

查询：
use oh;
select * from 保养信息
where 保养ID=(select 保养ID from 保养记录
where 设备ID='123456');
![](/1.jpg)
select * from 保养记录 where 设备ID='123456';
![](/2.jpg)
select * from 设备信息 where 设备ID='123456';
![](/3.jpg)
select * from 检修情况 where 设备ID="123456";
![](/4.jpg)
select 设备 ID from 设备信息
where 365-datediff(now(),(select 最近一次保养时间 from 设备信息))<10;
![](/5.jpg)
##ER
![](/6.jpg)



