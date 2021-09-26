

# Faster_Mean_Shift

Faster Mean-shift algorithm for accelerating the recurrent neural network (RNN) based pixel embedding framework for holistic cell segmentation and tracking. Here is a brief introduction on how to run it.


##  Environment
Win10

VS2019

Anacoda 2020.02

The packages requirement please see [requirements.txt](https://github.com/masqm/Faster_Mean_Shift/blob/master/requirements.txt "requirements.txt")


## Preparation
1. Download the datasets from the [celltracking challenge](http://www.celltrackingchallenge.net/) and extract them under an ***input_path***. 
2. Download the corresponding trained model from my [Google Driver](https://drive.google.com/drive/folders/1j1X0RFj0YOFYgKThM2p6HkEKxIQgtxo0?usp=sharing) and put them under a ***model_path***.
3. Create a folder for ***output_path***
4. Specify the ***dataset_name*** you want to run. Celltracking challenge provides 7 data-sets. We have tested four of them：

（1）DIC-C2DH-HeLa

（2）Fluo-N2DH-GOWT1

（3）Fluo-N2DH-SIM+

（4）PhC-C2DH-U373

For more data-set and model information, you can read our [paper](https://doi.org/10.1016/j.media.2021.102048) (arxiv:[paper](https://arxiv.org/abs/2007.14283)). The RNN frameworks are proposed by [Payer et al.](https://www.sciencedirect.com/science/article/pii/S136184151930057X?via%3Dihub)

## Modify Variable
In order to run the program, you need to modify some variables in [segment_and_track.py](https://github.com/masqm/Faster_Mean_Shift/blob/master/bin/segment_and_track.py "segment_and_track.py")

    input_image_folder = input_path		#eg."E:/code/data/myfile/01"
    
    output_folder = output_path 		#eg."E:/code/data/myfile/out"
    
    model_file_name = model_path		#eg."E:/code/data/myfile/01_model/DIC-C2DH-HeLa"
    
    dataset_name = dataset_name 		#eg."DIC-C2DH-HeLa"

## Testing
We provided a vs2019 project file for testing. You can run the program by executing the modified segment_and_track.py. We evaluated the time consumption and GPU memory requirement of the program. Please read our [paper](https://doi.org/10.1016/j.media.2021.102048) for specific performance data. 

When you run the program, the result will be output in the ***output_path*** folder.  The result is a 16-bit mask, and the pixel value represents the label. If the result needs to be visualized in this step, you should assign a visible label for each pixel. A simple Histogram Equalization is recommended. We provide a [result check tool](https://github.com/masqm/Faster-Mean-Shift/blob/master/bin/postprocess.py) for specific output result based on automatic histogram equalization in OpenCV. And you can use Photoshop to complete batch processing.

If you want to dye grayscale images, you can use [tif_process.py](https://github.com/masqm/Faster-Mean-Shift/blob/master/bin/color/tif_process.py "tif_process.py") and [color.py](https://github.com/masqm/Faster-Mean-Shift/blob/master/bin/color/color.py). The first program will dye the grayscale picture into color. The second picture will dye the picture generated by the gpu into the same color as the cpu according to the location

## Migration Algorithm
You are very welcome to use our faster mean-shift algorithm to develop your program. The entire algorithm is based on the following two files:

[mean_shift_cosine_gpu.py](https://github.com/masqm/Faster_Mean_Shift/blob/master/utils/mean_shift_cosine_gpu.py "mean_shift_cosine_gpu.py")

[batch_seed.py](https://github.com/masqm/Faster_Mean_Shift/blob/master/utils/batch_seed.py "batch_seed.py")

An example of how to using the algorithm is given below：

    #Import our algorithm
    from  utils.mean_shift_cosine_gpu  import  MeanShiftCosine
    
    #Configuration
    cluster = MeanShiftCosine(bandwidth=0.1, cluster_all=True, GPU=True)
    
    #Clustering the vectors. x is input vectors. The algorithm will cluster them.
    cluster.fit(x)
    
    #Obtain results
    labels  =  cluster.labels_

If you encounter any problem or find a bug during using, you are very welcome to contact me by (Mengyang.Zhao.TH@dartmouth.edu). If you use this code for your research, please cite our [paper](https://doi.org/10.1016/j.media.2021.102048). Thanks!
