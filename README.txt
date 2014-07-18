See http://commons.apache.org/pool/ for additional and 
up-to-date information on Apache Commons Pool.

注意：
    1. commons-pool 是一个[对象池/object-pooling]不是一个[线程池]

对象池运行策略：

    min_idle : 连接池最少(保持)空闲连接数
    (有定时器会定期检测池子的最小连接数，保证空闲数)

    max_idel : 连接池中保持最多的空闲连接数
    (每次将用过的连接重新入池时，若空闲连接数>max_idel，则该连接会被直接废弃)

    max_total : 连接池最多连接数
    (当激活数达到max_total后池子不在创建新对象，新请求会被阻塞直到有连接被释放，
    或者达到等待时间max_wait抛出异常)

    max_wait : 当pool耗尽时，再次向pool获取对象时最大的等待时间
    超过该时间抛出异常

    MIN_EVICTABLE_IDLE_TIME_MILLIS : 对象能够在池中保持空闲的最小时间


    对空闲线程的管理:
    每隔30s进行空闲线程管理(驱除/创建)操作：
        每次对连接池 部分 空闲对象进行测试，
        若连接对象的空闲时间 > 最小空闲时间(MIN_EVICTABLE_IDLE_TIME_MILLIS)，
        则将其废弃。

        然后检测连接池对象数，若对象数<min_idle，则重新补充到min_idle
        （当连接池超的对象>max_total，不再进行补充)

    当到30s进行扫描时，若在该时刻附近有对象被借出，则定时器会补充到20，
    从而出现21的情况。。。这个21应该不是"pool有对象仍创建新对象的bug"


    numTestsPerEvictionRun
    每次进行驱除测试时,测试池中对象的数量：测试连接池所有的空闲对象

    removeAbandonedTimeout
    若设置了removeAbandonedOnMaintenance，而且该对象的状态为ALLOCATED
    那么在这个对象超过removeAbandonedTimeout 将被删除

