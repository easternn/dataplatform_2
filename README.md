# Развертывание YARN

## Поднятие Веб интерфейсов описано в репозитории с ДЗ3

## Шаг 0. Требования

- HDFS Запущен через пользователя hadoop. Все дальнейшие инструкции должны быть выполнены из под данного пользователя.
- В переменную окружения PATH добавлены пути ```/home/hadoop/hadoop-3.4.0/sbin```, ```/home/hadoop/hadoop-3.4.0/bin```

## Шаг 1: Настройка названий нод, чтобы не вводить каждый раз ip адрес. 
В файл /etc/hosts на team@team-5-jn, team@team-5-nn, team@team-5-dn-00, team@team-5-dn-01 вставить следующие строчки в конец файла:
```
192.168.1.22 team-5-jn
192.168.1.23 team-5-nn
192.168.1.24 team-5-dn-0 
192.168.1.25 team-5-dn-1
```

## Шаг 2. Конфигурация
Все конфигурационные файлы лежат в директории
```bash
~/hadoop-3.4.0/etc/hadoop
```

1. На team-5-nn, team-5-dn-0, team-5-dn-1
в конфигурационный файл mapred-site.xml нужно добавить следующую информацию в блок ```<configuration></configuration>```, который изначально пустой.

```xml
<configuration>
	<property>
		<name>mapreduce.framework.name</name>
		<value>yarn</value>
	</property>
	<property>
		<name>mapreduce.application.classpath</name>
		<value>$HADOOP_HOME/share/hadoop/mapreduce/*:$HADOOP_HOME/share/hadoop/mapreduce/lib/*</value>
	</property>
</configuration>
```

2. На team-5-nn, team-5-dn-0, team-5-dn-1
в конфигурационный файл yarn-site.xml нужно добавить следующую информацию в блок ```<configuration></configuration>```, который изначально пустой.

```xml
<configuration>
	<property>
		<name>yarn.nodemanager.aux-services</name>
		<value>mapreduce_shuffle</value>
	</property>
	<property>
		<name>yarn.nodemanager.env-whitelist</name>
		<value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_HOME,PATH,LANG,TZ,HADOOP_MAPRED_HOME</value>
	</property>
</configuration>

```

## Шаг 3. Запуск YARN
На team-5-nn из директории
```bash
~/hadoop-3.4.0
```
выполнить следующую команду
```bash
/sbin/start-yarn.sh
```

## Шаг 4. Запуск historyserver
На team-5-nn из директории
```bash
~/hadoop-3.4.0
```
выполнить следующую команду
```bash
mapred --daemon start historyserver
```

## Шаг 4. Остановка всех сервисов

- Остановка History Server. На team-5-nn выполнить команду ```mapred --daemon stop historyserver``` Из директории ```~/hadoop-3.4.0```
- Остановка YARN. На team-5-nn выполнить команду ```./sbin/stop-yarn.sh``` Из директории ```~/hadoop-3.4.0```
- Остановка HDFS. На team-5-nn выполнить команду ```./sbin/stop-dfs.sh``` Из директории ```~/hadoop-3.4.0```
