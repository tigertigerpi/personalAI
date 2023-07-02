# chatgpt-retrieval

# 原版教程

Simple script to use ChatGPT on your own files.

Here's the [YouTube Video](https://youtu.be/9AXP7tCI9PI).

## Installation

Install [Langchain](https://github.com/hwchase17/langchain) and other required packages.
```
pip install langchain openai chromadb tiktoken unstructured
```
Modify `constants.py.default` to use your own [OpenAI API key](https://platform.openai.com/account/api-keys), and rename it to `constants.py`.

Place your own data into `data/data.txt`.

## Example usage
Test reading `data/data.txt` file.
```
> python chatgpt.py "what is my dog's name"
Your dog's name is Sunny.
```

Test reading `data/cat.pdf` file.
```
> python chatgpt.py "what is my cat's name"
Your cat's name is Muffy.
```

视频教程：[Using ChatGPT with YOUR OWN Data. This is magical. (LangChain OpenAI API) - YouTube](https://www.youtube.com/watch?v=9AXP7tCI9PI)
图文教程：[GitHub - techleadhd/chatgpt-retrieval](https://github.com/techleadhd/chatgpt-retrieval)

# 自制中文教程

# 太长看简版：

这是一个比较直接的安装指南。根据这个指南，你需要执行以下步骤：
1. **安装必要的包**：在命令行中输入并运行以下命令来安装必要的包：

   ```
   pip install langchain openai chromadb tiktoken unstructured
   ```
   
   请确保你的 Python 环境是最新的，并且 pip 也是最新版本。

2. **设置 OpenAI API 密钥**：修改 `constants.py.default` 文件，用你的 OpenAI API 密钥替换默认的密钥。然后将 `constants.py.default` 重命名为 `constants.py`（和chatgpt.py放在同一文件夹下）。

   你需要找到一行 `APIKEY = "<your OpenAI API key>"` 的代码，并将 `<your OpenAI API key>` 替换为你的 OpenAI API 密钥。例如，如果你的 API 密钥是 `'abcd1234'`，那么你需要将这行代码改为 `APIKEY = “abcd1234”`。

3. **提供数据**：将你的数据文件放入 `data/data.txt`。这个文件将被用作训练你的模型的数据。然后在chatgpt.py脚本中，将`loader = DirectoryLoader("my_data/")
更换为你自己的数据路径。（数据文件的文本格式，一般是.txt、.pdf或.json等）

4. **快乐玩耍**：进入命令行（win）或终端（Mac），输入这行命令试试看：

 ```
 python chatgpt.py  "hello"
```

记得测试私人数据库是否work。你可以先创建一个txt文件，然后把自己的名字写进去，输入问你名字的prompt。

在完成了上述步骤之后，你应该就可以正常运行你的代码了。如果你仍然遇到问题，你可能需要检查你的环境设置，或者寻求专业的帮助。

---

# 具体步骤

先配置环境，比如Python，如果你电脑里以前没安装过Python，需要进行安装。
然后进入命令行（win）或终端（Mac），分别输入
```
pip install langchain   
pip install openai 
pip install chromadb
pip install tiktoken
pip install unstructured
```

## Mac

在Mac端运行时，我遇到了如下问题：
一是，系统环境中缺少nltk（Natural Language Toolkit）的'punkt'资源（这是一个用于分词和句子分割的工具）。然后GPT建议我在Python环境下运行这段代码来安装：
```
import nltk
nltk.download('punkt')
```

二是，“有关SSL证书验证失败”。这可能是因为我Mac的Python环境中的SSL配置有问题。如果上述的'nltk'下载命令不能正常工作，并且仍然报告SSL错误，你可能需要修复你的Python环境的SSL设置。

GPT建议我尝试以下命令来更新Python环境中的certifi包，这是一个Python的SSL证书库：

```
pip install --upgrade certifi
```

但我按此操作后，还是不行。（我看到你已经成功地安装了最新版本的 certifi，但是你仍然遇到了 SSL 证书验证失败的问题）。

GPT再次建议我尝试使用以下命令（我的macOS上使用 Python 3.11）：

```
/Applications/Python\ 3.11/Install\ Certificates.command
```

这个命令尝试安装一组根证书，用于验证 SSL 连接。运行该命令之后，再次尝试下载 'punkt' 资源：

```
import nltk
nltk.download('punkt')
```

bingo!大功告成。

## Windows

在Windows中操作，最关键，也是最难搞的部分，应该就是安装这个五个Python包。

其他都还好，但chromadb这个包，由于中间要安装hnswlib。而安装hnswlib，要求C++编译器（Microsoft Visual C++ 14.0或者更新的版本）。这一步就折腾了好久。过程实在曲折，就简短说。

其中包括为了使用某博主讲的不用下大文件的编译器，而使用conda命令安装，还专门下载了1G多的conda命令运行软件Anaconda3，并将执行路径添加到系统环境变量中，但还是不行。
```
conda install libpython m2w64-toolchain -c msys2
```
![image](https://github.com/tigertigerpi/personalAI/assets/138350800/ddbd33fe-1339-4fd1-aa3a-f8c39e7e85f0)


没办法，只能老老实实根据命令行中的提示，去这个网站下载安装：[Microsoft C++ 生成工具 - Visual Studio](https://visualstudio.microsoft.com/zh-hans/visual-cpp-build-tools/)

```
Building wheel for hnswlib (pyproject.toml) ... error 
error: subprocess-exited-with-error 

× Building wheel for hnswlib (pyproject.toml) did not run successfully. 
│ exit code: 1 
╰─> [5 lines of output] 
   running bdist_wheel
   running build 
   running build_ext 
   building 'hnswlib' extension 
   error: Microsoft Visual C++ 14.0 or greater is required. Get it with "Microsoft C++ Build Tools": https://visualstudio.microsoft.com/visual-cpp-build-tools/ 
   [end of output] 
   
   note: This error originates from a subprocess, and is likely not a problem with pip. 
   ERROR: Failed building wheel for hnswlib 
   Failed to build hnswlib 
   ERROR: Could not build wheels for hnswlib, which is required to install pyproject.toml-based projects
```

但是，我下载安装了vs_BuildTools.exe后，没效果。问了ChatGPT好几遍也没结果，最后是搜到stack overflow去，才看到了还需要下载一个个别组件和一个Windows SDK。具体可以去原网址看看：[python - ERROR: Could not build wheels for hnswlib, which is required to install pyproject.toml-based projects - Stack Overflow](https://stackoverflow.com/questions/73969269/error-could-not-build-wheels-for-hnswlib-which-is-required-to-install-pyprojec)

![image](https://github.com/tigertigerpi/personalAI/assets/138350800/4485c41a-99bd-4330-8a93-a846312fb1a9)


在安装这两个组件时，看到预估空间挺大，大概五六个G吧。所以想把安装位置换到D盘。但怎么搞都不成功，最后查到微软官方的教程，才知道要更改安装路径，得第一次才可以。指路→[选择安装位置 - Visual Studio (Windows) | Microsoft Learn](https://learn.microsoft.com/zh-cn/visualstudio/install/change-installation-locations?view=vs-2022)

![image](https://github.com/tigertigerpi/personalAI/assets/138350800/9e29fc72-1788-4ae1-bf81-64feb781b0d7)


不得已，又将vs_BuildTools卸载后，重装。然后，更改安装路径，把上面提到的那两个组件选择下载，关机睡觉。第二天醒来，已经下好了。

下好后，再试：
```
pip install chormadb
```

bingo!大功告成。

另外，在安装unstructured这个包时，如果遇到这个提示：
```
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts. onnxruntime 1.15.1 requires numpy>=1.24.2, but you have numpy 1.23.5 which is incompatible.
```

可以不用管，或者简单使用这个命令来升级numpy就可以了：
```
pip install --upgrade numpy
or
pip install numpy==1.24.2
```

---

在配置API key时，我误将chatgpt.py代码中"OPENAI_API_KEY"，认为是需要填上OpenAI的API Key。后来程序报错问GPT才发现。可见我有多菜、多不仔细。

```
os.environ["OPENAI_API_KEY"] = constants.APIKEY
```

实际上，配置API Key作者说得很清楚，就是新建一个Python脚本，打开这里面（[GitHub - techleadhd/chatgpt-retrieval](https://github.com/techleadhd/chatgpt-retrieval)）的constants.py.default代码，复制，粘贴，然后代码中的“<your OpenAI API key>”（注意“<和>”也要替换），用你自己的API Key替换，最后保存为“constants.py”。

![image](https://github.com/tigertigerpi/personalAI/assets/138350800/9233361b-56c6-4832-aa88-0d27e331f925)
![image](https://github.com/tigertigerpi/personalAI/assets/138350800/e98afc5a-b0e1-4098-b7ab-aa20c3910f57)


特别注意！这个脚本要和chatgpt.py这个脚本在同一文件夹（目录）下。之所以要这样，就我有限的Python知识来说，应该是在chatgpt.py脚本中，利用这段代码,，导入了“constants”：

```
import constants
os.environ["OPENAI_API_KEY"] = constants.APIKEY
```

----

最后，如果提示这个错误：

```
SyntaxError: (unicode error) 'unicodeescape' codec can't decode bytes in position 2-3: truncated \UXXXXXXXX escape
```
可以通过在每个反斜杠前再添加一个反斜杠来转义它们。

最后的最后，祝你成功~
