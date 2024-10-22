# Развертывание YARN

## Шаг 0. Требования

- HDFS Запущен через пользователя hadoop. Все дальнейшие инструкции должны быть выполнены из под данного пользователя. В нашем сеттинге team-5-nn - машина содержащая демона NameNode, team-5-dn-0 и team-5-dn-1 - машины содержащие только демонов DataNode. В вашем сеттинге могут быть другие названия. Также в блоке 4 используются их внутренние IP аресса: team-5-nn (192.168.1.23), team-5-dn-0 (192.168.1.24), team-5-dn-1 (192.168.1.25). В вашем сеттинге могут быть другие ip адреса. В остальном инструкции никак не меняются для других пользователей.

## Шаг 1. Конфигурация
Все конфигурационные файлы лежат в директории
```bash
~/hadoop-3.4.0/etc/hadoop
```

1. На team-5-nn, team-5-dn-00, team-5-dn-01
В конфигурационный файл mapred-site.xml нужно добавить следующую информацию в блок ```<configuration></configuration>```, который изначально пустой.

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
В конфигурационный файл yarn-site.xml нужно добавить следующую информацию в блок ```<configuration></configuration>```, который изначально пустой.

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

## Шаг 3. Запуск historyserver
На team-5-nn выполнить команду

```bash
mapred --daemon start historyserver
```
Из директории
```bash
~/hadoop-3.4.0
```

## Шаг 4. Доступ к UI

На локальной машине выполнить команду:
```bash
ssh -L 9881:192.168.1.23:9864 -L 9882:192.168.1.24:9864 -L 9883:192.168.1.25:9864 -L 9884:192.168.1.23:9870  -L 9885:192.168.1.23:19888 team@176.109.91.7
```

Доступ к веб-интерфейсам:

- Data Node (team-5-nn): <http://localhost:9881>
- Data Node (team-5-dn-00): <http://localhost:9882>
- Data Node (team-5-dn-01): <http://localhost:9883>
- Name Node (team-5-nn): <http://localhost:9884>
- History Server (team-5-nn): <http://localhost:9885>

## Шаг 5. Остановка всех сервисов

- Остановка History Server. На team-5-nn выполнить команду ```mapred --daemon stop historyserver``` Из директории ```~/hadoop-3.4.0```
- Остановка YARN. На team-5-nn выполнить команду ```./sbin/stop-yarn.sh``` Из директории ```~/hadoop-3.4.0```
- Остановка HDFS. На team-5-nn выполнить команду ```./sbin/stop-dfs.sh``` Из директории ```~/hadoop-3.4.0```
