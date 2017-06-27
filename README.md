# python 库的学习记录
***
splendid python library

## argparse
</br>
**一、简介：**
- `argparse`是python用于解析命令行参数和选项的标准模块，用于代替已经过时的optparse模块。argparse模块的作用是用于解析命令行参数，例如`python parseTest.py input.txt output.txt --user=name --port=8080`

**二、使用步骤：**
- 1：import argparse
- 2：parser = argparse.ArgumentParser()
- 3：parser.add_argument()
- 4：parser.parse_args()

  *解释：首先导入该模块；然后创建一个解析对象；然后向该对象中添加你要关注的命令行参数和选项，每一个add_argument方法对应一个你要关注的参数或选项；最后调用parse_args()方法进行解析；解析成功之后即可使用，下面简单说明一下步骤2和3。*
 
**三、方法ArgumentParser**(prog=None, usage=None,description=None, epilog=None, parents=[],formatter_class=argparse.HelpFormatter, prefix_chars='-',fromfile_prefix_chars=None, argument_default=None,conflict_handler='error', add_help=True)

这些参数都有默认值，当调用parser.print_help()或者运行程序时由于参数不正确(此时python解释器其实也是调用了pring_help()方法)时，会打印这些描述信息，一般只需要传递description参数，如上。

**四、方法add_argument**(name or flags...[, action][, nargs][, const][, default][, type][, choices][, required][, help][, metavar][, dest])
其中：
- name or flags：命令行参数名或者选项，如上面的address或者-p,--port.其中命令行参数如果没给定，且没有设置defualt，则出错。但是如果是选项的话，则设置为None
- nargs：命令行参数的个数，一般使用通配符表示，其中，'?'表示只用一个，'\*'表示0到多个，'+'表示至少一个
- default：默认值
- type：参数的类型，默认是字符串string类型，还有float、int等类型
- help：和ArgumentParser方法中的参数作用相似，出现的场合也一致
 
最常用的地方就是这些，其他的可以参考官方文档。下面给出一个例子，基本包括了常见的情形： 
```
import argparse

def parse_args():
    description = usage: %prog [options] poetry-file

This is the Slow Poetry Server, blocking edition.
Run it like this:

  python slowpoetry.py <path-to-poetry-file>

If you are in the base directory of the twisted-intro package,
you could run it like this:

  python blocking-server/slowpoetry.py poetry/ecstasy.txt

to serve up John Donne's Ecstasy, which I know you want to do.


    parser = argparse.ArgumentParser(description = description)

    help = The addresses to connect.
    parser.add_argument('addresses',nargs = '*',help = help)

    help = The filename to operate on.Default is poetry/ecstasy.txt
    parser.add_argument('filename',help=help)

    help = The port to listen on. Default to a random available port.
    parser.add_argument('-p',--port', type=int, help=help)

    help = The interface to listen on. Default is localhost.
    parser.add_argument('--iface', help=help, default='localhost')

    help = The number of seconds between sending bytes.
    parser.add_argument('--delay', type=float, help=help, default=.7)

    help = The number of bytes to send at a time.
    parser.add_argument('--bytes', type=int, help=help, default=10)

    args = parser.parse_args();
    return args

if __name__ == '__main__':
    args = parse_args()

    for address in args.addresses:
        print 'The address is : %s .' % address

    print 'The filename is : %s .' % args.filename
    print 'The port is : %d.' % args.port
    print 'The interface is : %s.' % args.iface
    print 'The number of seconds between sending bytes : %f'% args.delay
    print 'The number of bytes to send at a time : %d.' % args.bytes</path-to-poetry-file>
```

运行该脚本：`python test.py --port 10000 --delay 1.2 127.0.0.1 172.16.55.67 poetry/ecstasy.txt `
输出为：
```
The address is : 127.0.0.1 .
The address is : 172.16.55.67 .
The filename is : poetry/ecstasy.txt .
The port is : 10000.
The interface is : localhost.
The number of seconds between sending bytes : 1.200000
The number of bytes to send at a time : 10.
```

**再例如：**

[命令行给出参数，运行图片分类https://github.com/zhudaoruyi/image_recognition_keras_jupyter/blob/master/classify.py](https://github.com/zhudaoruyi/image_recognition_keras_jupyter/blob/master/classify.py)

```
if __name__=="__main__":
    a = argparse.ArgumentParser()
    a.add_argument("--image", help="path to image")
    a.add_argument("--image_url", help="url to image")
    args = a.parse_args()

    if args.image is None and args.image_url is None:
        a.print_help()
        sys.exit(1)

    if args.image is not None:
        img = Image.open(args.image)
        preds = predict(model, img, target_size)
        plot_preds(img, preds)

    if args.image_url is not None:
        response = requests.get(args.image_url)
        img = Image.open(BytesIO(response.content))
        preds = predict(model, img, target_size)
        plot_preds(img, preds)
```
**再例如：**

[迁移学习（InceptionV3）+ 微调实现猫狗分类https://github.com/zhudaoruyi/cat_dog_classify/blob/master/fine-tune.py](https://github.com/zhudaoruyi/cat_dog_classify/blob/master/fine-tune.py)
```
if __name__=="__main__":
    a = argparse.ArgumentParser()
    a.add_argument("--train_dir")
    a.add_argument("--val_dir")
    a.add_argument("--epochs", default=EPOCHS)
    a.add_argument("--batch_size", default=BAT_SIZE)
    a.add_argument("--output_model_file", default="inceptionv3-ft.model")
    a.add_argument("--plot", action="store_true")
    args = a.parse_args()
    if args.train_dir is None or args.val_dir is None:
        a.print_help()
        sys.exit(1)
    if (not os.path.exists(args.train_dir)) or (not os.path.exists(args.val_dir)):
        print("directories do not exist")
        sys.exit(1)
```
