# HEVCAcceleration

Offical implementation of [Zhibo Chen*, Jun Shi, Weiping Li, “Learned Fast HEVC Intra Coding”, Vol.29, Issue.1, pp.5431-5446, IEEE Trans. on Image Processing, 2020.](https://arxiv.org/abs/2112.13309v2)

1. 网络模型使用方法：
	CNN模型基于深度学习框架TensorFlow，建议在ubuntu下运行，TensorFlow版本大于1.0均可，所需资源库在requirements.txt文件中给出，执行以下命令即可：
pip install -r requirements.txt
	网络模型分为两类，在ModeDecision及SizeDecision两个文件夹内。网络的输入为当前深度下的划分块的亮度分量，大小为64x64，32x32，16x16，8x8及4x4（此大小仅在模式选择任务上使用）。划分算法使用的是二分类模型，对输出值使用argmax操作，0代表不划分，1代表划分，而模式选择算法的输出节点为3个，使用argmax操作得到的值记为n，则对于深度0～2的块而言，表示MNRC值为n+1，对于深度3～4的块而言，MNRC值为3*n+2。输出文件的命名方式需要基于序列名及块大小，例如对于Traffic序列（yuv名）的64x64块，文件命名为Traffic_64.txt，其中输出值按照HEVC块递归顺序进行排序。

2. 编码器使用方法：
	编码器均基于HM 16.9进行修改编译获得，建议运行于Windows 10平台。我们提供了三个编码器，分别是快速模式选择、快速块划分及整体框架的exe文件。其运行指令与HM标准exe指令相同，另需使用神经网络生成相应的Flag文件，并于上级目录下建立两个文件夹，分别命名为IntraFlag和DepthFlag，将Flag文件放入其中即可。假设当前文件夹名为bin，下面给出样例：
coding
├── bin
├── IntraFlag
└── DepthFlag
注意Flag文件需要按照划分深度依次存储，例如对于Traffic序列，在Depth-Flag文件夹下需要有4个文件，分别命名为Traffic_64.txt,Traffic_32.txt, Traffic_16.txt及Traffic_8.txt。

3. EHIC数据集细节：
	数据集主要基于RAISE、DIV2K及UCID三个公开数据集，包含了各个分辨率下的自然场景图像。Data文件夹下已经按照HEVC中CTU划分方式提供了64x64大小的灰度块的mat文件，而Label文件夹下提供了快速划分及快速模式选择两项任务的标签，划分任务的标签值为0或1，代表了划分或不划分，而模式选择任务上的标签对应MNRC值，深度0～2时取值为1～3，而深度3～4时取值为1～8。
![image](https://github.com/USTC-IMCL/HEVCAcceleration/assets/113843291/d7a43c74-e695-4f0b-8f4e-b9366c342eb1)
