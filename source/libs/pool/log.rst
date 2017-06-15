Log
===

Log连接池仅供syslog类型使用，长连接连的是我们内部的flume日志系统（未开源）。运维同事在每台生产机器上部署了
flume 的
client端，所以长连接连接本地就可以了。开发环境的话可以考虑部署到一台公用的开发机器上。

配置示例
--------

配置文件路径：resource/config/${ENV}/connection/syslog.php

.. code:: php

    <?php

    return [
        'syslog_default' => [
            'engine'=> 'syslog',
            'host' => '127.0.0.1',
            'port' => '5140',
            'timeout' => 5000,
            'persistent' => true,
            'pool' => [
                'keeping-sleep-time' => 10000,
                'init-connection' => 2,
            ],
        ],
        //zan框架内置配置，无需设置，直接使用
        'zan_framework' => [
            'engine'=> 'syslog',
            'host' => '127.0.0.1',
            'port' => '5140',
            'timeout' => 5000,
            'persistent' => true,
            'pool' => [
                'keeping-sleep-time' => 10000,
                'init-connection' => 1,
                'maximum-connection-count' => 3,
                'minimum-connection-count' => 1,
            ],
        ],

    ];

上述配置数组中的key与log.php的syslog
url配置有关。采用syslog的日志文件输出，其url配置的path字段与key对应，如log.php配置为

.. code:: php

    return [
        'default' => 'syslog://info/syslog_default?module=default',
    ]

在日志输出时就会对应发送到上述连接池中的连接了。

