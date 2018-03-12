## Setup

Install TensorFlow package
```
sudo pip install tensorflow
```

Clone this repo

```
git clone https://github.com/anfeng/Models TFmodels-anfeng
pushd TFmodels-anfeng/official/mnist/
```

## MNIST with local file storage

```
python mnist.py --train_epochs 2 --data_dir=/tmp/local_dataset_storage
```

Executing the above command will uses datasets located at file:/tmp/local_dataset_storage.
You should see the following logs. 

```
INFO:tensorflow:Using default config.
INFO:tensorflow:Using config: {'_save_checkpoints_secs': 600, '_session_config': None, '_keep_checkpoint_max': 5, '_task_type': 'worker', '_global_id_in_cluster': 0, '_is_chief': True, '_cluster_spec': <tensorflow.python.training.server_lib.ClusterSpec object at 0x11a01a5d0>, '_evaluation_master': '', '_save_checkpoints_steps': None, '_keep_checkpoint_every_n_hours': 10000, '_service': None, '_num_ps_replicas': 0, '_tf_random_seed': None, '_master': '', '_num_worker_replicas': 1, '_task_id': 0, '_log_step_count_steps': 100, '_model_dir': '/tmp/mnist_model', '_save_summary_steps': 100}
INFO:tensorflow:Calling model_fn.
INFO:tensorflow:Done calling model_fn.
INFO:tensorflow:Create CheckpointSaverHook.
INFO:tensorflow:Graph was finalized.
2018-03-08 12:39:55.849663: I tensorflow/core/platform/cpu_feature_guard.cc:140] Your CPU supports instructions that this TensorFlow binary was not compiled to use: AVX2 FMA
INFO:tensorflow:Restoring parameters from /tmp/mnist_model/model.ckpt-9231
INFO:tensorflow:Running local_init_op.
INFO:tensorflow:Done running local_init_op.
INFO:tensorflow:Saving checkpoints for 9232 into /tmp/mnist_model/model.ckpt.
INFO:tensorflow:train_accuracy = 1.0
INFO:tensorflow:loss = 0.008601271, step = 9232
INFO:tensorflow:global_step/sec: 7.3835
...
INFO:tensorflow:train_accuracy = 0.9975 (12.993 sec)
INFO:tensorflow:loss = 0.00022402673, step = 12132 (12.993 sec)
INFO:tensorflow:Saving checkpoints for 12231 into /tmp/mnist_model/model.ckpt.
INFO:tensorflow:Loss for final step: 0.0030540098.
INFO:tensorflow:Calling model_fn.
INFO:tensorflow:Done calling model_fn.
INFO:tensorflow:Starting evaluation at 2018-03-08-20:47:40
INFO:tensorflow:Graph was finalized.
INFO:tensorflow:Restoring parameters from /tmp/mnist_model/model.ckpt-12231
INFO:tensorflow:Running local_init_op.
INFO:tensorflow:Done running local_init_op.
INFO:tensorflow:Finished evaluation at 2018-03-08-20:47:45
INFO:tensorflow:Saving dict for global step 12231: accuracy = 0.993, global_step = 12231, loss = 0.0205939

Evaluation results:
	{'loss': 0.0205939, 'global_step': 12231, 'accuracy': 0.993}
```

## MNIST with S3 data storage

Let's execute mnist.py with datasets on S3. The following command uses a S3 bucket 'mnist-andyf' which consists of 4 files (t10k-images-idx3-ubyte, t10k-labels-idx1-ubyte,	train-images-idx3-ubyte, train-labels-idx1-ubyte) which are created from the original mnist datafiles (http://yann.lecun.com/exdb/mnist/).

```
python mnist.py --train_epochs 2 --data_dir=s3://mnist-andyf/
```

Application logs will look like below. It's very similar to the log from local storage, but has log entries indicating interaction with S3.
```
INFO:tensorflow:Using default config.
INFO:tensorflow:Using config: {'_save_checkpoints_secs': 600, '_session_config': None, '_keep_checkpoint_max': 5, '_task_type': 'worker', '_global_id_in_cluster': 0, '_is_chief': True, '_cluster_spec': <tensorflow.python.training.server_lib.ClusterSpec object at 0x1181b2210>, '_evaluation_master': '', '_save_checkpoints_steps': None, '_keep_checkpoint_every_n_hours': 10000, '_service': None, '_num_ps_replicas': 0, '_tf_random_seed': None, '_master': '', '_num_worker_replicas': 1, '_task_id': 0, '_log_step_count_steps': 100, '_model_dir': '/tmp/mnist_model', '_save_summary_steps': 100}
INFO:tensorflow:Calling model_fn.
INFO:tensorflow:Done calling model_fn.
INFO:tensorflow:Create CheckpointSaverHook.
INFO:tensorflow:Graph was finalized.
2018-03-08 12:53:25.715238: I tensorflow/core/platform/cpu_feature_guard.cc:140] Your CPU supports instructions that this TensorFlow binary was not compiled to use: AVX2 FMA
INFO:tensorflow:Restoring parameters from /tmp/mnist_model/model.ckpt-12231
INFO:tensorflow:Running local_init_op.
INFO:tensorflow:Done running local_init_op.
2018-03-08 12:53:26.289192: I tensorflow/core/platform/s3/aws_logging.cc:54] Initializing config loader against fileName /Users/andyfeng//.aws/config and using profilePrefix = 1
2018-03-08 12:53:26.289214: I tensorflow/core/platform/s3/aws_logging.cc:54] Initializing config loader against fileName /Users/andyfeng//.aws/credentials and using profilePrefix = 0
2018-03-08 12:53:26.289223: I tensorflow/core/platform/s3/aws_logging.cc:54] Setting provider to read credentials from /Users/andyfeng//.aws/credentials for credentials file and /Users/andyfeng//.aws/config for the config file , for use with profile default
2018-03-08 12:53:26.289240: I tensorflow/core/platform/s3/aws_logging.cc:54] Creating HttpClient with max connections2 and scheme http
2018-03-08 12:53:26.289259: I tensorflow/core/platform/s3/aws_logging.cc:54] Initializing CurlHandleContainer with size 2
2018-03-08 12:53:26.289270: I tensorflow/core/platform/s3/aws_logging.cc:54] Creating Instance with default EC2MetadataClient and refresh rate 900000
2018-03-08 12:53:26.289282: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
2018-03-08 12:53:26.289617: I tensorflow/core/platform/s3/aws_logging.cc:54] Initializing CurlHandleContainer with size 25
2018-03-08 12:53:26.289709: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
2018-03-08 12:53:26.289892: I tensorflow/core/platform/s3/aws_logging.cc:54] Pool grown by 2
2018-03-08 12:53:26.289900: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
2018-03-08 12:53:26.507072: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
...
2018-03-08 12:53:36.182124: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
2018-03-08 12:53:36.259741: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
2018-03-08 12:53:36.259840: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
2018-03-08 12:53:36.318168: I tensorflow/core/kernels/data/shuffle_dataset_op.cc:94] Filling up shuffle buffer (this may take a while): 29425 of 50000
...
2018-03-08 12:53:42.096904: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
2018-03-08 12:53:42.169423: I tensorflow/core/kernels/data/shuffle_dataset_op.cc:130] Shuffle buffer filled.
INFO:tensorflow:Saving checkpoints for 12232 into /tmp/mnist_model/model.ckpt.
INFO:tensorflow:train_accuracy = 1.0
INFO:tensorflow:loss = 0.0004476022, step = 12232
2018-03-08 12:53:42.504698: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
2018-03-08 12:53:42.504788: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
...
2018-03-08 12:54:03.165975: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
2018-03-08 12:54:03.166120: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
INFO:tensorflow:global_step/sec: 4.70606
INFO:tensorflow:train_accuracy = 1.0 (21.250 sec)
INFO:tensorflow:loss = 0.0018535889, step = 12332 (21.249 sec)
INFO:tensorflow:global_step/sec: 8.0616
INFO:tensorflow:train_accuracy = 0.99333334 (12.405 sec)
INFO:tensorflow:loss = 0.050834674, step = 12432 (12.405 sec)
INFO:tensorflow:global_step/sec: 7.65609
INFO:tensorflow:train_accuracy = 0.995 (13.061 sec)
INFO:tensorflow:loss = 0.0038747806, step = 12532 (13.061 sec)
INFO:tensorflow:global_step/sec: 7.88616
INFO:tensorflow:train_accuracy = 0.994 (12.680 sec)
INFO:tensorflow:loss = 0.02531346, step = 12632 (12.680 sec)
INFO:tensorflow:global_step/sec: 7.9263
INFO:tensorflow:train_accuracy = 0.99333334 (12.616 sec)
INFO:tensorflow:loss = 0.020912828, step = 12732 (12.616 sec)
2018-03-08 12:55:07.223812: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
2018-03-08 12:55:07.223936: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
...
2018-03-08 12:55:17.166981: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
2018-03-08 12:55:17.167081: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
2018-03-08 12:55:17.223808: I tensorflow/core/kernels/data/shuffle_dataset_op.cc:94] Filling up shuffle buffer (this may take a while): 36807 of 50000
...
2018-03-08 12:55:20.436743: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
2018-03-08 12:55:20.530028: I tensorflow/core/kernels/data/shuffle_dataset_op.cc:130] Shuffle buffer filled.
INFO:tensorflow:global_step/sec: 3.81471
INFO:tensorflow:train_accuracy = 0.99285716 (26.214 sec)
INFO:tensorflow:loss = 0.025604341, step = 12832 (26.214 sec)
2018-03-08 12:55:20.671028: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
2018-03-08 12:55:20.671133: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
...
2018-03-08 12:55:35.970914: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
2018-03-08 12:55:35.971021: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
INFO:tensorflow:global_step/sec: 6.35159
INFO:tensorflow:train_accuracy = 0.99375 (15.744 sec)
INFO:tensorflow:loss = 0.0039912243, step = 12932 (15.744 sec)
...
INFO:tensorflow:global_step/sec: 7.67367
INFO:tensorflow:train_accuracy = 0.99333334 (13.032 sec)
INFO:tensorflow:loss = 0.008774721, step = 13332 (13.032 sec)
INFO:tensorflow:Saving checkpoints for 13431 into /tmp/mnist_model/model.ckpt.
INFO:tensorflow:Loss for final step: 0.0003605815.
INFO:tensorflow:Calling model_fn.
INFO:tensorflow:Done calling model_fn.
INFO:tensorflow:Starting evaluation at 2018-03-08-20:56:41
INFO:tensorflow:Graph was finalized.
INFO:tensorflow:Restoring parameters from /tmp/mnist_model/model.ckpt-13431
INFO:tensorflow:Running local_init_op.
INFO:tensorflow:Done running local_init_op.
2018-03-08 12:56:41.675521: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
2018-03-08 12:56:41.675660: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
2018-03-08 12:56:41.874690: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
2018-03-08 12:56:41.874891: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
2018-03-08 12:56:41.915393: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
...
2018-03-08 12:56:48.696771: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
2018-03-08 12:56:48.696862: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
INFO:tensorflow:Finished evaluation at 2018-03-08-20:56:48
INFO:tensorflow:Saving dict for global step 13431: accuracy = 0.9924, global_step = 13431, loss = 0.022081725

Evaluation results:
	{'loss': 0.022081725, 'global_step': 13431, 'accuracy': 0.9924}
```

## MNIST with S3 data storage + local cache

Let's avoid the unwanted S3 interactions by leveraging Dataset::cache() API. 
You could have dataset cached in a file or in memory.

To cache in file,
```
python mnist.py --train_epochs 2 --data_dir=s3://mnist-andyf/ --cache_spec "/tmp/cache"

```
The above command will create cache files such as:
```
-rw-r--r--  1 andyfeng  wheel  188400000 Mar  8 13:07 /tmp/cache.data-00000-of-00001
-rw-r--r--  1 andyfeng  wheel    2973037 Mar  8 13:07 /tmp/cache.index
```

To cache in memory,
```
python mnist.py --train_epochs 2 --data_dir=s3://mnist-andyf/ --cache_spec ""

```

Whatever options you chose for caching, execution log will has much less S3 related entries. Each file is fetched once only. 
```
INFO:tensorflow:Using default config.
INFO:tensorflow:Using config: {'_save_checkpoints_secs': 600, '_session_config': None, '_keep_checkpoint_max': 5, '_task_type': 'worker', '_global_id_in_cluster': 0, '_is_chief': True, '_cluster_spec': <tensorflow.python.training.server_lib.ClusterSpec object at 0x1169f3210>, '_evaluation_master': '', '_save_checkpoints_steps': None, '_keep_checkpoint_every_n_hours': 10000, '_service': None, '_num_ps_replicas': 0, '_tf_random_seed': None, '_master': '', '_num_worker_replicas': 1, '_task_id': 0, '_log_step_count_steps': 100, '_model_dir': '/tmp/mnist_model', '_save_summary_steps': 100}
mnist dataset will be cached: /tmp/cache
INFO:tensorflow:Calling model_fn.
INFO:tensorflow:Done calling model_fn.
INFO:tensorflow:Create CheckpointSaverHook.
INFO:tensorflow:Graph was finalized.
2018-03-08 13:07:04.067194: I tensorflow/core/platform/cpu_feature_guard.cc:140] Your CPU supports instructions that this TensorFlow binary was not compiled to use: AVX2 FMA
INFO:tensorflow:Restoring parameters from /tmp/mnist_model/model.ckpt-13431
INFO:tensorflow:Running local_init_op.
INFO:tensorflow:Done running local_init_op.
2018-03-08 13:07:04.653394: I tensorflow/core/platform/s3/aws_logging.cc:54] Initializing config loader against fileName /Users/andyfeng//.aws/config and using profilePrefix = 1
2018-03-08 13:07:04.653415: I tensorflow/core/platform/s3/aws_logging.cc:54] Initializing config loader against fileName /Users/andyfeng//.aws/credentials and using profilePrefix = 0
2018-03-08 13:07:04.653425: I tensorflow/core/platform/s3/aws_logging.cc:54] Setting provider to read credentials from /Users/andyfeng//.aws/credentials for credentials file and /Users/andyfeng//.aws/config for the config file , for use with profile default
2018-03-08 13:07:04.653443: I tensorflow/core/platform/s3/aws_logging.cc:54] Creating HttpClient with max connections2 and scheme http
2018-03-08 13:07:04.653463: I tensorflow/core/platform/s3/aws_logging.cc:54] Initializing CurlHandleContainer with size 2
2018-03-08 13:07:04.653476: I tensorflow/core/platform/s3/aws_logging.cc:54] Creating Instance with default EC2MetadataClient and refresh rate 900000
2018-03-08 13:07:04.653487: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
2018-03-08 13:07:04.653695: I tensorflow/core/platform/s3/aws_logging.cc:54] Initializing CurlHandleContainer with size 25
2018-03-08 13:07:04.653753: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
2018-03-08 13:07:04.653881: I tensorflow/core/platform/s3/aws_logging.cc:54] Pool grown by 2
2018-03-08 13:07:04.653888: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
2018-03-08 13:07:04.869550: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
...
2018-03-08 13:07:14.630628: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
2018-03-08 13:07:14.630727: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
2018-03-08 13:07:14.787575: I tensorflow/core/kernels/data/shuffle_dataset_op.cc:94] Filling up shuffle buffer (this may take a while): 22737 of 50000
...
2018-03-08 13:07:26.188547: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
2018-03-08 13:07:26.188653: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
2018-03-08 13:07:26.259330: I tensorflow/core/kernels/data/shuffle_dataset_op.cc:130] Shuffle buffer filled.
INFO:tensorflow:Saving checkpoints for 13432 into /tmp/mnist_model/model.ckpt.
INFO:tensorflow:train_accuracy = 1.0
INFO:tensorflow:loss = 0.0041377377, step = 13432
2018-03-08 13:07:26.585608: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
2018-03-08 13:07:26.585695: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
...
2018-03-08 13:07:45.706867: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
2018-03-08 13:07:45.706974: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
INFO:tensorflow:global_step/sec: 5.04865
INFO:tensorflow:train_accuracy = 1.0 (19.808 sec)
INFO:tensorflow:loss = 0.0061312034, step = 13532 (19.808 sec)
INFO:tensorflow:global_step/sec: 7.71079
INFO:tensorflow:train_accuracy = 1.0 (12.969 sec)
INFO:tensorflow:loss = 0.001475783, step = 13632 (12.969 sec)
INFO:tensorflow:global_step/sec: 7.56166
INFO:tensorflow:train_accuracy = 1.0 (13.225 sec)
INFO:tensorflow:loss = 0.0038984874, step = 13732 (13.225 sec)
INFO:tensorflow:global_step/sec: 7.62984
INFO:tensorflow:train_accuracy = 1.0 (13.106 sec)
INFO:tensorflow:loss = 0.0032960018, step = 13832 (13.106 sec)
INFO:tensorflow:global_step/sec: 7.69967
INFO:tensorflow:train_accuracy = 1.0 (12.988 sec)
INFO:tensorflow:loss = 0.00064339786, step = 13932 (12.988 sec)
INFO:tensorflow:global_step/sec: 7.63981
INFO:tensorflow:train_accuracy = 1.0 (13.089 sec)
INFO:tensorflow:loss = 0.0033457251, step = 14032 (13.089 sec)
INFO:tensorflow:global_step/sec: 7.61815
INFO:tensorflow:train_accuracy = 1.0 (13.127 sec)
INFO:tensorflow:loss = 0.0014657128, step = 14132 (13.127 sec)
INFO:tensorflow:global_step/sec: 7.68306
INFO:tensorflow:train_accuracy = 1.0 (13.016 sec)
INFO:tensorflow:loss = 0.014155699, step = 14232 (13.016 sec)
INFO:tensorflow:global_step/sec: 7.64424
INFO:tensorflow:train_accuracy = 1.0 (13.082 sec)
INFO:tensorflow:loss = 0.001353823, step = 14332 (13.082 sec)
INFO:tensorflow:global_step/sec: 7.71382
INFO:tensorflow:train_accuracy = 1.0 (12.964 sec)
INFO:tensorflow:loss = 0.0016930476, step = 14432 (12.964 sec)
INFO:tensorflow:global_step/sec: 7.65943
INFO:tensorflow:train_accuracy = 1.0 (13.056 sec)
INFO:tensorflow:loss = 0.002438021, step = 14532 (13.056 sec)
INFO:tensorflow:Saving checkpoints for 14631 into /tmp/mnist_model/model.ckpt.
INFO:tensorflow:Loss for final step: 0.0007241091.
INFO:tensorflow:Calling model_fn.
INFO:tensorflow:Done calling model_fn.
INFO:tensorflow:Starting evaluation at 2018-03-08-21:10:10
INFO:tensorflow:Graph was finalized.
INFO:tensorflow:Restoring parameters from /tmp/mnist_model/model.ckpt-14631
INFO:tensorflow:Running local_init_op.
INFO:tensorflow:Done running local_init_op.
2018-03-08 13:10:10.201408: I tensorflow/core/platform/s3/aws_logging.cc:54] Found secret key
2018-03-08 13:10:10.201542: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
...
2018-03-08 13:10:17.637554: I tensorflow/core/platform/s3/aws_logging.cc:54] Connection has been released. Continuing.
INFO:tensorflow:Finished evaluation at 2018-03-08-21:10:17
INFO:tensorflow:Saving dict for global step 14631: accuracy = 0.9933, global_step = 14631, loss = 0.021325123

Evaluation results:
	{'loss': 0.021325123, 'global_step': 14631, 'accuracy': 0.9933}
```

When you use file cache, later jobs will be able to reuse your cache. The following execution, for example, doesn't have any AWS related entries during training phase.

```
$ python mnist.py --train_epochs 2 --data_dir=s3://mnist-andyf/ --cache_spec "/tmp/cache"
INFO:tensorflow:Using default config.
INFO:tensorflow:Using config: {'_save_checkpoints_secs': 600, '_session_config': None, '_keep_checkpoint_max': 5, '_task_type': 'worker', '_global_id_in_cluster': 0, '_is_chief': True, '_cluster_spec': <tensorflow.python.training.server_lib.ClusterSpec object at 0x11d2a3210>, '_evaluation_master': '', '_save_checkpoints_steps': None, '_keep_checkpoint_every_n_hours': 10000, '_service': None, '_num_ps_replicas': 0, '_tf_random_seed': None, '_master': '', '_num_worker_replicas': 1, '_task_id': 0, '_log_step_count_steps': 100, '_model_dir': '/tmp/mnist_model', '_save_summary_steps': 100}
mnist dataset will be cached: /tmp/cache
INFO:tensorflow:Calling model_fn.
INFO:tensorflow:Done calling model_fn.
INFO:tensorflow:Create CheckpointSaverHook.
INFO:tensorflow:Graph was finalized.
2018-03-12 11:38:37.671015: I tensorflow/core/platform/cpu_feature_guard.cc:140] Your CPU supports instructions that this TensorFlow binary was not compiled to use: AVX2 FMA
INFO:tensorflow:Restoring parameters from /tmp/mnist_model/model.ckpt-23031
INFO:tensorflow:Running local_init_op.
INFO:tensorflow:Done running local_init_op.
INFO:tensorflow:Saving checkpoints for 23032 into /tmp/mnist_model/model.ckpt.
INFO:tensorflow:train_accuracy = 1.0
INFO:tensorflow:loss = 0.0071553458, step = 23032
INFO:tensorflow:global_step/sec: 8.61573
INFO:tensorflow:train_accuracy = 0.995 (11.607 sec)
INFO:tensorflow:loss = 0.054765362, step = 23132 (11.607 sec)
INFO:tensorflow:global_step/sec: 8.52085
INFO:tensorflow:train_accuracy = 0.99666667 (11.736 sec)
INFO:tensorflow:loss = 4.2091096e-06, step = 23232 (11.736 sec)
INFO:tensorflow:global_step/sec: 8.38499
INFO:tensorflow:train_accuracy = 0.9975 (11.926 sec)
INFO:tensorflow:loss = 0.00023929411, step = 23332 (11.926 sec)
INFO:tensorflow:global_step/sec: 8.25801
INFO:tensorflow:train_accuracy = 0.996 (12.109 sec)
INFO:tensorflow:loss = 0.022046257, step = 23432 (12.109 sec)
INFO:tensorflow:global_step/sec: 8.28354
INFO:tensorflow:train_accuracy = 0.99666667 (12.072 sec)
INFO:tensorflow:loss = 0.0005147812, step = 23532 (12.072 sec)
INFO:tensorflow:global_step/sec: 8.13739
INFO:tensorflow:train_accuracy = 0.9957143 (12.289 sec)
INFO:tensorflow:loss = 0.010904511, step = 23632 (12.289 sec)
INFO:tensorflow:global_step/sec: 7.53667
INFO:tensorflow:train_accuracy = 0.99625 (13.268 sec)
INFO:tensorflow:loss = 0.0012038928, step = 23732 (13.268 sec)
INFO:tensorflow:global_step/sec: 8.12338
INFO:tensorflow:train_accuracy = 0.99666667 (12.310 sec)
INFO:tensorflow:loss = 0.0002907067, step = 23832 (12.310 sec)
INFO:tensorflow:global_step/sec: 7.95268
INFO:tensorflow:train_accuracy = 0.997 (12.574 sec)
INFO:tensorflow:loss = 0.002992282, step = 23932 (12.574 sec)
INFO:tensorflow:global_step/sec: 6.95121
INFO:tensorflow:train_accuracy = 0.99727273 (14.386 sec)
INFO:tensorflow:loss = 0.00066570134, step = 24032 (14.386 sec)
INFO:tensorflow:global_step/sec: 7.15812
INFO:tensorflow:train_accuracy = 0.9975 (13.970 sec)
INFO:tensorflow:loss = 0.000105894964, step = 24132 (13.970 sec)
INFO:tensorflow:Saving checkpoints for 24231 into /tmp/mnist_model/model.ckpt.
INFO:tensorflow:Loss for final step: 0.00016191343.
```



