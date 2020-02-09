# 下载Python（Centos）

### 1）安装依赖包

```
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel libffi-devel
```

### 2）下载python

https://www.python.org/ftp/python/

这里我们选择下载3.7.0。

### 3）解压

`tar -zxvf Python-3.7.0.tgz `

### 4）建立一个空文件夹，存放python3

`mkdir /usr/local/python/python3`

### 5）执行配置文件，编译，编译安装

```
cd Python-3.7.0
./configure --prefix=/usr/local/python/python3
make && make install
```

### 6）建立软连接　

`ln -s /usr/local/python/python3/bin/python3.7 /usr/bin/python3`

`ln -s /usr/local/python/python3/bin/pip3.7 /usr/bin/pip3`

### 7）测试

`python3`

# 安装magenta

### 1）升级pip3

`pip3 install --upgrade pip`

### 2）下载前置东西

```
pip3 install --ignore-installed wrapt
pip3 install --ignore-installed  setuptools
pip3 install tensorflow
```

### 3）下载Magenta

`pip3 install magenta`

# 由于种种原因，放弃Centos，转战ubuntu18

## 下载python

### 1）前置

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
sudo apt-get install build-essential python-dev python-setuptools python-pip python-smbus
sudo apt-get install build-essential libncursesw5-dev libgdbm-dev libc6-dev
sudo apt-get install zlib1g-dev libsqlite3-dev tk-dev
sudo apt-get install libssl-dev openssl
sudo apt-get install libffi-dev
sudo apt-get install libbz2-dev
sudo apt-get install libsndfile1
```

### 2）下载python3

```
wget http://www.python.org/ftp/python/3.7.0/Python-3.7.0.tgz
tar -xvzf Python-3.7.0.tgz
cd Python-3.7.0
./configure --with-ssl
make
sudo make install
```

## 安装magenta环境

```
curl https://raw.githubusercontent.com/tensorflow/magenta/master/magenta/tools/magenta-install.sh > /tmp/magenta-install.sh
bash /tmp/magenta-install.sh
```

## 运行

### 编辑shell脚本

```
BUNDLE_PATH=/root/python_demo/magenta-test/drum_kit_rnn.mag # <absolute path of .mag file>
CONFIG=drum_kit  # <one of 'one_drum' or 'drum_kit', matching the bundle>
#
drums_rnn_generate --config=${CONFIG} \
    --bundle_file=${BUNDLE_PATH} \
    --output_dir=/root/python_demo/magenta-test/music \
    --num_outputs=10 --num_steps=128 --primer_drums="[(36,)]"
```

相应的目录上创建相应的文件，其中.mag为magenta的github官方库中下载。也可以直接使用命令下载：

`wget -o xxx https://.......`

具体链接在github项目中的每个modules文件的.md开头中。

赋予权限后，运行编辑好的脚本文件，将会自动在output_dir目录生成.midi文件，是不是很神奇！虽然并不是很好听（默默吐槽）。

## 用自己的midi文件进行训练

我们来训练melody类型的~首先通过midishow平台，通过评论拿积分进行下载大量的midi流行音乐，然后上传至ubuntu，进行训练。

### 1）midi文件转换成.tfrecord文件

创建并编辑脚本文件，transfer.sh：

```
INPUT_DIRECTORY=<folder containing MIDI and/or MusicXML files. can have child folders.>

# TFRecord file that will contain NoteSequence protocol buffers.
SEQUENCES_TFRECORD=/tmp/notesequences.tfrecord

convert_dir_to_note_sequences \
  --input_dir=$INPUT_DIRECTORY \
  --output_file=$SEQUENCES_TFRECORD \
  --recursive
```

首行写上midi文件总目录，在第二行即SEQUENCES_TFRECORD写上生成文件安放的目录，赋予执行权限，运行。

### 2）生成SequenceExamples

```
melody_rnn_create_dataset \
--config=<one of 'basic_rnn', 'mono_rnn', lookback_rnn', or 'attention_rnn'> \
--input=/tmp/notesequences.tfrecord \
--output_dir=/tmp/melody_rnn/sequence_examples \
--eval_ratio=0.10
```

其中配置我们选`attention_rnn`，input写上我们之前生成的.tfrecord文件目录，output_dir写上要放置的目录。

### 3）训练

```
melody_rnn_train \
--config=attention_rnn \
--run_dir=/tmp/melody_rnn/logdir/run1 \
--sequence_example_file=/tmp/melody_rnn/sequence_examples/training_melodies.tfrecord \
--hparams="batch_size=64,rnn_layer_sizes=[64,64]" \
--num_training_steps=20000
```

run_dir自己配置即可，是运行的目录，然后其他参数仿照上面。训练次数为20000次，赋予权限后再此运行。

### 4）评估

脚本代码如下：

```
melody_rnn_train \
--config=attention_rnn \
--run_dir=/tmp/melody_rnn/logdir/run1 \
--sequence_example_file=/tmp/melody_rnn/sequence_examples/eval_melodies.tfrecord \
--hparams="batch_size=64,rnn_layer_sizes=[64,64]" \
--num_training_steps=20000 \
--eval
```

注意，这里的sequence_example_file是sequence_examples目录下的另一个文件。

可以运行tensorboard来观看模型的效果。

```
tensorboard --logdir=/tmp/melody_rnn/logdir
```

使用ip:6006进行访问。

### 自动生成音乐

编辑脚本：

```
melody_rnn_generate \
--config=attention_rnn \
--run_dir=/tmp/melody_rnn/logdir/run1 \
--output_dir=/tmp/melody_rnn/generated \
--num_outputs=10 \
--num_steps=128 \
--hparams="batch_size=64,rnn_layer_sizes=[64,64]" \
--primer_melody="[60]"
```

生成目录自己替换即可。最后就会得到一份midi文件，将其传至本地，就可以听取啦！！！虽然我很想吐槽这个音乐是真的迷。

# 总结

magenta是个很有趣的tensorflow项目，内置了很多功能，在我的学习的过程中，也遭遇了非常多的坎坷，但也正是因为这些坎坷，才能变强（秃）。

