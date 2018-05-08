### c++ frcnn caffe gpu windows

## Introduction
The windows C ++ version of faster-rcnn is based on two versions:  
### 一: [happynear's windows-caffe](https://github.com/happynear/caffe-windows)  
This version is happynear's windows caffe version    
### 二: [huaze555's windows-caffe-frcnn version](https://github.com/huaze555/windows-caffe-faster-rcnn)  
This is the huaze555's windows c ++ version of faster-rcnn

###
caffe 1.0
cuda 8.0
cuDNN 5.1
/*Miniconda 2.7 64-bit*/
windows 10 x64
visual studio 2015
nv gtx 1050

## reference

一: [huaze555's windows-caffe-frcnn-github](https://github.com/huaze555/windows-caffe-faster-rcnn)
VS2013 x64 release mode
Microsoft's windows-caffe + D-X-Y's caffe-faster-rcnn version
二: [huaze555's windows-caffe-frcnn-blog1](http://www.cnblogs.com/love6tao/p/5706830.html)
三: [huaze555's windows-caffe-frcnn-blog2](https://blog.csdn.net/zxj942405301/article/details/78602671)
四: [D-X-Y's caffe-faster-rcnn linux version](https://github.com/D-X-Y/caffe-faster-rcnn/tree/dev) 

## compile

### 1. fix "./Windows/CommonSettings.props" from happynear's step

### 2. copy following flies from huaze555 to happynear
/include/caffe/api/*
/include/caffe/FRCNN/*
/include/cub/*
/src/caffe/api/*
/src/caffe/FRCNN/*

### 3. add all files in "./Windows/libcaffe/libcaffe.vcxproj" when open "./Windows/caffe.sln"
put all include files into include part
/include/caffe/api/*
/include/caffe/FRCNN/*
/include/cub/cub.cuh
put all src files into src part except cu files
/src/caffe/api/*
/src/caffe/FRCNN/*
put all cu files into libcaffe part
*.cu

### 4. fix "./src/caffe/caffe.proto" from huaze555 to happynear
add
```
// Xuanyi Add For FRCNN config file path
optional string config = 20 [default = ""];
```
like
```
message WindowDataParameter {
  // Specify the data source.
  optional string source = 1;
  // For data pre-processing, we can do simple scaling and subtracting the
  // data mean, if provided. Note that the mean subtraction is always carried
  // out before scaling.
  optional float scale = 2 [default = 1];
  optional string mean_file = 3;
  // Specify the batch size.
  optional uint32 batch_size = 4;
  // Specify if we would like to randomly crop an image.
  optional uint32 crop_size = 5 [default = 0];
  // Specify if we want to randomly mirror data.
  optional bool mirror = 6 [default = false];
  // Foreground (object) overlap threshold
  optional float fg_threshold = 7 [default = 0.5];
  // Background (non-object) overlap threshold
  optional float bg_threshold = 8 [default = 0.5];
  // Fraction of batch that should be foreground objects
  optional float fg_fraction = 9 [default = 0.25];
  // Amount of contextual padding to add around a window
  // (used only by the window_data_layer)
  optional uint32 context_pad = 10 [default = 0];
  // Mode for cropping out a detection window
  // warp: cropped window is warped to a fixed size and aspect ratio
  // square: the tightest square around the window is cropped
  optional string crop_mode = 11 [default = "warp"];
  // cache_images: will load all images in memory for faster access
  optional bool cache_images = 12 [default = false];
  // append root_folder to locate images
  optional string root_folder = 13 [default = ""];
  // Xuanyi Add For FRCNN config file path
  optional string config = 20 [default = ""];
}
```



### 4.1 issue
```
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(22): error C2065: 'ROIMaskPoolingParameter': undeclared identifier
  ..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(21): note: while compiling class template member function 'void caffe::ROIMaskPoolingLayer<float>::LayerSetUp(const std::vector<caffe::Blob<Dtype> *,std::allocator<_Ty>> &,const std::vector<_Ty,std::allocator<_Ty>> &)'
          with
          [
              Dtype=float,
              _Ty=caffe::Blob<float> *
          ]
  ..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(142): note: see reference to class template instantiation 'caffe::ROIMaskPoolingLayer<float>' being compiled
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(22): error C2146: syntax error: missing ';' before identifier 'roi_mask_pool_param'
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(22): error C2065: 'roi_mask_pool_param': undeclared identifier
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(22): error C2039: 'roi_mask_pooling_param': is not a member of 'caffe::LayerParameter'
  D:\caffe-windows-ms\include\caffe/proto/caffe.pb.h(3001): note: see declaration of 'caffe::LayerParameter'
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(23): error C2065: 'roi_mask_pool_param': undeclared identifier
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(23): error C2228: left of '.pooled_h' must have class/struct/union
  ..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(23): note: type is 'unknown-type'
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(23): error C2512: 'google::CheckOpString': no appropriate default constructor available
  D:\caffe-windows-ms\windows\thirdparty\Glog\include\glog/logging.h(584): note: see declaration of 'google::CheckOpString' (compiling source file ..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp)
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(25): error C2065: 'roi_mask_pool_param': undeclared identifier
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(25): error C2228: left of '.pooled_w' must have class/struct/union
  ..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(25): note: type is 'unknown-type'
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(25): error C2512: 'google::CheckOpString': no appropriate default constructor available
  D:\caffe-windows-ms\windows\thirdparty\Glog\include\glog/logging.h(584): note: see declaration of 'google::CheckOpString' (compiling source file ..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp)
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(27): error C2065: 'roi_mask_pool_param': undeclared identifier
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(27): error C2228: left of '.pooled_h' must have class/struct/union
  ..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(27): note: type is 'unknown-type'
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(28): error C2065: 'roi_mask_pool_param': undeclared identifier
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(28): error C2228: left of '.pooled_w' must have class/struct/union
  ..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(28): note: type is 'unknown-type'
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(29): error C2065: 'roi_mask_pool_param': undeclared identifier
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(29): error C2228: left of '.spatial_scale' must have class/struct/union
  ..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(29): note: type is 'unknown-type'
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(30): error C2065: 'roi_mask_pool_param': undeclared identifier
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(30): error C2228: left of '.half_part' must have class/struct/union
  ..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(30): note: type is 'unknown-type'
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(31): error C2065: 'roi_mask_pool_param': undeclared identifier
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(31): error C2228: left of '.roi_scale' must have class/struct/union
  ..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(31): note: type is 'unknown-type'
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(32): error C2065: 'roi_mask_pool_param': undeclared identifier
..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(32): error C2228: left of '.mask_scale' must have class/struct/union
  ..\..\src\caffe\FRCNN\roi_mask_pooling_layer.cpp(32): note: type is 'unknown-type'
  frcnn_file.cpp
..\..\src\caffe\FRCNN\smooth_L1_loss_layer.cpp(15): error C2065: 'SmoothL1LossParameter': undeclared identifier
  ..\..\src\caffe\FRCNN\smooth_L1_loss_layer.cpp(14): note: while compiling class template member function 'void caffe::SmoothL1LossLayer<float>::LayerSetUp(const std::vector<caffe::Blob<Dtype> *,std::allocator<_Ty>> &,const std::vector<_Ty,std::allocator<_Ty>> &)'
          with
          [
              Dtype=float,
              _Ty=caffe::Blob<float> *
          ]
  ..\..\src\caffe\FRCNN\smooth_L1_loss_layer.cpp(67): note: see reference to class template instantiation 'caffe::SmoothL1LossLayer<float>' being compiled
..\..\src\caffe\FRCNN\smooth_L1_loss_layer.cpp(15): error C2146: syntax error: missing ';' before identifier 'loss_param'
..\..\src\caffe\FRCNN\smooth_L1_loss_layer.cpp(15): error C2065: 'loss_param': undeclared identifier
..\..\src\caffe\FRCNN\smooth_L1_loss_layer.cpp(15): error C2039: 'smooth_l1_loss_param': is not a member of 'caffe::LayerParameter'
  D:\caffe-windows-ms\include\caffe/proto/caffe.pb.h(3001): note: see declaration of 'caffe::LayerParameter'
..\..\src\caffe\FRCNN\smooth_L1_loss_layer.cpp(16): error C2065: 'loss_param': undeclared identifier
..\..\src\caffe\FRCNN\smooth_L1_loss_layer.cpp(16): error C2228: left of '.sigma' must have class/struct/union
  ..\..\src\caffe\FRCNN\smooth_L1_loss_layer.cpp(16): note: type is 'unknown-type'
```

### solution
```
/*put in message LayerParameter*/
//Bruce Add For Faster RCNN
  optional ROIMaskPoolingParameter roi_mask_pooling_param = 242;
  optional SmoothL1LossParameter smooth_l1_loss_param = 243;

/*add the end of caffe.proto*/
//Bruce Add For Faster RCNN
message ROIMaskPoolingParameter {
  // Pad, kernel size, and stride are all given as a single value for equal
  // dimensions in height and width or as Y, X pairs.
  optional uint32 pooled_h = 1 [default = 0]; // The pooled output height
  optional uint32 pooled_w = 2 [default = 0]; // The pooled output width
  // Multiplicative spatial scale factor to translate ROI coords from their
  // input scale to the scale used when pooling
  optional float spatial_scale = 3 [default = 1];
  optional int32 half_part = 4 [default = 0];
  optional float roi_scale = 5 [default = 1];
  optional float mask_scale = 6 [default = 0];
}

message SmoothL1LossParameter {
  // SmoothL1Loss(x) =
  //   0.5 * (sigma * x) ** 2    -- if x < 1.0 / sigma / sigma
  //   |x| - 0.5 / sigma / sigma -- otherwise
  optional float sigma = 1 [default = 1];
}
```



### 5. build "./windows/caffe.sln"



### just only build huaze555's issue
### issue 1
D:\windows-caffe-faster-rcnn\include\caffe/util/cudnn.hpp(114): error : too few arguments in function call
  softmax_loss_ohem_layer.cu
### solution
This is because cudnn version is too high, there is a function was changed to need to pass 9 parameters.
change cudnn version from v6.0 to v5.1

### issue 2
  BinplaceCudaDependencies : Copy cudart*.dll, cublas*dll, curand*.dll to output.
  C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0\bin\cudart32_80.dll
  C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0\bin\cudart64_80.dll
  複製了         2 個檔案。
  C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0\bin\cublas64_80.dll
  複製了         1 個檔案。
  C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0\bin\curand64_80.dll
  複製了         1 個檔案。
  BinplaceCudaDependencies : Copy cudnn*.dll to output.
  系統找不到指定的路徑。
C:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\V140\Microsoft.CppCommon.targets(133,5): error MSB3073: The command ""D:\windows-caffe-faster-rcnn\windows\\scripts\BinplaceCudaDependencies.cmd" "C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0\bin" "C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0" false true "D:\windows-caffe-faster-rcnn\windows\..\Build\x64\Release\"
C:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\V140\Microsoft.CppCommon.targets(133,5): error MSB3073: :VCEnd" exited with code 1.
========== Rebuild All: 0 succeeded, 1 failed, 0 skipped ==========
### solution
<CuDnnPath></CuDnnPath> don't need any path