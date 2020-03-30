# ANNs
SIMURG - A CAD Tool for ANN Design in Hardware

AIM
---
Aim of the SIMURG code is to describe the ANN designs under different design architectures in Verilog and generate the test-bench for simulation and verification. SIMURG can describe the ANN design in hardware under parallel and two time-multiplexed architectures where multiply-accumulate (MAC) blocks are used to share the computing resources. Under one time-multiplexed architecture, called SMAC_NEURON, each neuron at each layer is realized using a single MAC block and under the other one, called SMAC_ANN, the whole ANN is realized using a single MAC block. Moreover, we introduce the multiplierless realization of ANN designs using the fewest number of addition/subtraction operations found by prominent previously proposed algorithms. 

USAGE
-----
The usage of the SIMURG code can be given as follows:

Usage:       perl simurg.pl <File_Name> -ibw=<int> -obw=<int> -quan=<int> -tech=1-7 -act=lin/relu/hsig/htanh/satlin -test=0/1         -not=<int> -dvf=<File_Name> -nnit -creg -rep=binary/csd -aim=area/delay -wout -h       
  
File_Name:   Name of the file including the ANN weigth and bias values                                                                                                                                                
-ibw:        Bitwidth of inputs, by default it is 8                                                                                                                                                                   
-obw:        Bitwidth of outputs, by default it is 8                                                                                                                                                                  
-quan:       Quantization value for weight and bias values, by default it is 8. It can be greater than or equal to 0                                              
-tech:       Technique to design the ANN, by default it is 1                                                                                                                                                          
                1: Realization of ANN under the parallel architecture - constant multiplications are defined in a behavioral fashion (par_beh)                                                                        
                2: Realization of ANN under the parallel architecture - constant multiplications are implemented under the shift-adds architecture using multiple constant multiplciation blocks (par_mcm)            
                3: Realization of ANN under the parallel architecture - constant multiplications are implemented under the shift-adds architecture using constant array vector multiplication blocks (par_cavm)       
                4: Realization of ANN under the parallel architecture - constant multiplications are implemented under the shift-adds architecture using constant matrix vector multiplication blocks (par_cmvm)      
                5: Realization of ANN under the SMAC_NEURON architecture - constant multiplications are defined in a behavioral fashion (smac_neuron_beh)                                                             
                6: Realization of ANN under the SMAC_NEURON architecture - constant multiplications are implemented under the shift-adds architecture using multiple constant multiplication blocks (smac_neuron_mcm) 
                7: Realization of ANN under the SMAC_ANN architecture - constant multiplications are defined in a behavioral fashion (smac_ann)                                                                       
-act:        Activation function of neurons on each layer separated by a slash, by default it is htanh at the hidden layers and lin at the output layer                                                               
                0: lin  - linear                                                                                                                                                                                      
                1: relu - rectified linear unit                                                                                                                                                                       
                2: hsig - hard sigmoid fuction                                                                                                                                                                        
                3: htanh - hard hyperbolic tangent                                                                                                                                                                    
                4: satlin - saturating linear transfer function                                                                                                                                                       
-test:       Technique to verify the ANN design, by default it is 0                                                                                                                                                   
                0: Random                                                                                                                                                                                             
                1: Directed (using the test data)                                                                                                                                                                     
-not:        Number of test patterns to be used in the random verification technique, by default it is 10000                                                                                                          
-dvf:        Name of the file including the test values to be used in the directed verification technique                                                                                                             
-nnit:       Assumes that the inputs are not normalized during training, by default the normalization of inputs is assumed                                                                                            
-creg:       Adds flip-flops to the outputs of a parallel design obtained between techniques 1 and 4, by default it does not                                                                                          
-rep:        Number representation used in the CMVM algorithm, by default it is CSD                                                                                                                                   
-aim:        Optimization aim in the MCM, CAVM, or CMVM algorithm, by default it is area                                                                                                                              
-wout:       Quantized weight and bias values in each layer are written into a file with an "iw" extension, by default they are not                                                                                   
-h:          Prints this screen                                                                                                                                                                                       
Description: Automatically generates the Verilog design and test-bench codes for the ANN implementation                                                                                                               
Examples:    perl simurg.pl examples\ANN_16_16_10_10.w -quan=7 -tech=7 -act=htanh/htanh/hsig                                                                                                                          
             perl simurg.pl examples\ANN_16_10_10.w -quan=7 -tech=5 -act=htanh/hsig                                                                                                                                   
             perl simurg.pl examples\ANN_16_10.w -quan=5 -tech=4 -act=hsig -rep=csd -aim=area                                                                                                                         
             perl simurg.pl examples\ANN_16_10.w -quan=5 -tech=1 -creg -wout -test=0 -dvf=test_data/pen_test_norm2_q8.conv

INPUT FORMAT
------------
The weight and bias values of the ANN design are found using a training algorithm and stored in a file under a pre-determined format. A simple example for an ANN design with 3 inputs, first layer with 4 neurons, and the second layer with 2 neurons are given below. Other examples can be found under the "examples" directory. 

[3 4 2]
Layer #1
0.8921465240443548 -0.377793223098982 -1.6271769231468052 -1.2628203855845146 
-1.4352316501874032 -1.7617135662984922 1.8556483130527284 -2.586953547094126
0.17267326469207 -0.0900296649171026 -1.5999492555675363 1.9277605190763356
-1.0491819706342647 -0.9727538037898444 -0.8585108605405252 2.206534487617965
Layer #2
-0.7251862849545974 2.0063227161393566 0.0435958215774369 -0.9272709898893278 -1.539158427084033 
0.0515105462661097 -0.0358411654569719 -0.7016437732974243 -1.8504751546092744 0.2920622834161278 

Note that the values at each line under the related layer denotes the weights of a neuron associated with the input, except the last value which actually stands for the bias associated with the neuron.

DEPENDICES
----------
For the multiplierless design of ANN, it needs the algorithms designed for the multiplierless design of Multiple Constant Multiplication (MCM), Constant Array Vector Multiplication (CAVM) and Constant Matrix Vector Multiplication (CMVM) blocks and the tools that describe these realizations in hardware. All these algorithms developed in MATLAB can be found under the 
matlab_support directory. The main files under the related directories are named as given below:

cmvm: cmvm_algo.m
cmvm_dc: cmvm_dc_algo.m
cavm: cavm_algo.m
mcm: mcm_algo.m
cmvm_high2low: cmvm_high2low.m
cavm_high2low: cavm_high2low.m
mcm_high2low: cmvm_high2low.m

These main files need to be generated as executables so that it can be run within the SIMURG Perl code. It is done by simply running the "mcc -m <MATLAB_FILE.m>" command at the MATLAB prompt. After the executable files are generated, the "paths.pl" 
file should include their locations. Moreover, these algorithms need a number of look-up tables which should be located under the same directory with the SIMURG Perl code. They are named as min_depth, nzd_cost, and scm_cost and are included in the luts directory.

Note also that these MATLAB files are standalone executables.

OUTPUT
------
SIMURG generates the Verilog design and test-bench codes of the ANN implementation under the different design architectures.

NOTES
-----
SIMURG has been tested under operating system Windows 10 using the Strawberry Perl (http://strawberryperl.com/) and the MATLAB executables generated using the MATLAB 2018a.

CONTACT
-------
Please contact Levent Aksoy (aksoyl@itu.edu.tr) if you have any trouble running the codes and suggestions and/or comments.
