# ANNs
SIMURG - A CAD Tool for ANN Design in Hardware

Aim of the SIMURG tool is to describe the ANN designs under different design architectures in Verilog and generate the test-bench for simulation and verification. SIMURG can describe the ANN design in hardware under parallel and two time-multiplexed architectures where multiply-accumulate (MAC) blocks are used to share the computing resources. Under one time-multiplexed architecture, called SMAC_NEURON, each neuron at each layer is realized using a single MAC block and under the other one, called SMAC_ANN, the whole ANN is realized using a single MAC block. Moreover, we introduce the multiplierless realization of ANN designs using the fewest number of addition/subtraction operations found by prominent previously proposed algorithms. 

Details on the SIMURG tool can be found in its README file.

For citation, please use the following: 

@INPROCEEDINGS{aksoy20,

  author={Aksoy, Levent and Parvin, Sajjad and Nojehdeh, Mohammadreza Esmali and Altun, Mustafa},
  
  booktitle={IEEE International Symposium on Circuits and Systems (ISCAS)}, 
  
  title={Efficient Time-Multiplexed Realization of Feedforward Artificial Neural Networks}, 
  
  year={2020},
  
  pages={1-5},
  
  doi={10.1109/ISCAS45731.2020.9181002}
}
