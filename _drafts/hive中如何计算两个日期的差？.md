###HIVE中如何计算两个日期的差？
找来找去hive中最适合做这件事的就是datadiff函数，可是这个函数貌似只接收yyyy-mm-dd格式的时间，而我的日期格式是yyyymmdd。所以要想用这个函数还得折腾一遍。。。
select
	datediff(from_unixtime(unix_timestamp('20150501', 'yyyyMMdd'),'yyyy-MM-dd'),from_unixtime(unix_timestamp('20150510', 'yyyyMMdd'),'yyyy-MM-dd'))
这个返回的是-9