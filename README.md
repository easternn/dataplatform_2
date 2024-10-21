# Развертывание YARN

## Шаг 0. Требования

- HDFS Запущен через пользователя hadoop на всех нодах. Все дальнейшие инструкции должны быть выполнены из под данного пользователя

## Шаг 1. Конфигурация
Все конфигурационные файлы лежат в директории
```bash
~/hadoop-3.4.0/etc/hadoop
```

1. На team-5-nn, team-5-dn-00, team-5-dn-01
В конфигурационный файл mapred-site.xml нужно добавить следующую информацию в блок <configuration></configuration>, который изначально пустой.

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

2. На team-5-nn, team-5-dn-00, team-5-dn-01
В конфигурационный файл yarn-site.xml нужно добавить следующую информацию в блок <configuration></configuration>, который изначально пустой.

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

## Шаг 2. Запуск YARN
На team-5-nn выполнить команду

```bash
./sbin/start-yarn.sh
```
Из директории
```bash
~/hadoop-3.4.0
```

## Шаг 3. Доступ к UI

На локальной машине выполнить команду:
```bash
ssh -L 9880:192.168.1.23:8088 -L 9881:192.168.1.23:8042 -L 9882:192.168.1.24:8042 -L 9883:192.168.1.25:8042 team@176.109.91.7
```

Доступ к веб-интерфейсам:

- Resource Manager (team-5-nn): <http://localhost:9880>
- Node Manager (team-5-nn): <http://localhost:9881>
- Node Manager (team-5-dn-00): <http://localhost:9882>
- Node Manager (team-5-dn-01): <http://localhost:9883>

## Шаг 4. Остановка

На team-5-nn выполнить команду

```bash
./sbin/stop-yarn.sh
```
Из директории
```bash
~/hadoop-3.4.0
```
