fluentd-initscript4debian
=========================

Fluentd START/STOP initscript for Debian/GNU Linux 6.0 (squeeze)

Overview
--------

Fluentdにはinitscriptが同梱されていないので、daemon運用するのが面倒。なので、それを楽にするために作りました。


QuickStart
----------

Debian GNU/Linux 6.0 (squeeze)

### install ruby ###

	$ sudo apt-get install ruby1.9.1-full make
	$ sudo update-alternatives --install /usr/bin/ruby ruby /usr/bin/ruby1.9.1 400 \
	       --slave /usr/share/man/man1/ruby.1.gz ruby.1.gz /usr/share/man/man1/ruby1.9.1.1.gz \
		   --slave /usr/bin/ri ri /usr/bin/ri1.9.1 --slave /usr/bin/irb irb /usr/bin/irb1.9.1
	$ sudo update-alternatives --install /usr/bin/gem gem /usr/bin/gem1.9.1 150
	$ sudo update-alternatives --config ruby
	$ sudo update-alternatives --config gem


### install Fluentd ###

	sudo gem install fluentd --bindir /usr/local/bin


### setup env. & install initscript ###

	$ sudo groupadd -r fluent
	$ sudo useradd -r -c "Fluent user" -g fluent -s /bin/bash -d /home/fluent fluent
	$ sudo mkdir -p /etc/fluent/ /var/run/fluent/ /var/log/fluent/ /home/fluent/
	$ sudo chown -R fluent:fluent /etc/fluent/ /var/run/fluent/ /var/log/fluent/ /home/fluent/
	$ sudo -u fluent fluentd --setup /etc/fluent/

	$ curl https://raw.github.com/ma2shita/fluentd-initscript4debian/master/fluentd | \
	  sudo sh -c "cat - > /etc/init.d/fluentd ; chmod 755 /etc/init.d/fluentd"
	$ sudo insserv -v -d /etc/init.d/fluentd

`/etc/init.d/fluentd`説明
--------------------------

fluentの実行コマンドは`/usr/local/bin`にインストールされている前提です。fluentのインストール時に`bindir`を指定するか、`/etc/init.d/fluentd`を調整してください。

fluentdの実行ユーザーは`fluent`です。pidファイルやログファイルの格納先のownerに気をつけてください。

末尾にdがついたり、つかなかったりしてます、ややこしくてごめんなさい。

[EoF]
