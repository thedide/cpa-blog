---
title: "How to Install Apache Zeppelin"
date: 2020-06-28T13:50:58Z
description: "Apache Zeppelin (web-based notebook for interactive data analytics) installation guide for Scala."
tags: ["Technology", "Apache Zeppelin", "How to", "Installation"]
---


## What is Zeppelin?

[Zeppelin](https://zeppelin.apache.org/) is a web-based interactive notebook for data processing (exploration, transformation, model building, etc.) and visualization.


## Where to download?

You can download the latest release of Zeppelin from [https://zeppelin.apache.org/download.html](https://zeppelin.apache.org/download.html).

## Which package should I download?

In this tutorial I will download the "Binary package with Spark interpreter and interpreter net-install script". If you are interested in interpreters other than Spark you can download the "Binary package with all interpreters" and if you are interested in building the tool from its source code you can download the source package.

## Step by step guide

### Download the package

```bash
cd /path/to/download/zeppelin
wget http://apache.claz.org/zeppelin/zeppelin-0.7.1/zeppelin-0.7.1-bin-netinst.tgz
```

### Extract the archive

```bash
tar xzf zeppelin-0.7.1-bin-netinst.tgz
```

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- cpa -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-2843564932689995"
     data-ad-slot="3526097725"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

### Configuring Zeppelin

```bash
cp conf/zeppelin-site.xml.template conf/zeppelin-site.xml
cp conf/zeppelin-env.sh.template conf/zeppelin-env.sh
```

find "zeppelin.server.port" and change the port number as you wish

edit zeppelin-env.sh and add the following lines:
export SPARK_HOME=/usr/local/spark
export SPARK_SUBMIT_OPTIONS="--driver-class-path /opt/hadoopgpl/lib/hadoop-lzo.jar"
