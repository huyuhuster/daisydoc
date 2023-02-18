# Python 算法开发

&emsp;&emsp;Python 形式的算法实现与C++形式类似，需要实现基类中的 3 个抽象方法，initialize()， execute() 和 finalize();下面以实现 tomopy 重建算法为例，具体实现如下：  

```python
from __future__ import print_function
import Daisy

# Inheritance the algorithm base class
class AlgTomopyRecon(Daisy.Base.DaisyAlg):


    def __init__(self, name):
        super().__init__(name)

    def initialize(self):
        # Get the DataStore
        self.data = self.get("DataStore").data()
        self.LogInfo("initialized, Tomopy Reconstruction")
        return True
    # The input of an algorithm generally include a set of input data objects, a set of calculated parameters, and a set of output data objects
    def execute(self, input_dataobj, theta, center, alg_type, output_dataobj):
        # Importing third-party Libraries
        import tomopy
        # Get the input data from the datastore via the key
        projs  = self.data[input_dataobj]
        thetas = self.data[theta]

        # Implementation of data processing logic
        dataobj = tomopy.recon(projs, thetas, center=center, algorithm=alg_type)
        # Register the output data back to the datastore, which can be accessed by other modules or saved to an output file  
        self.data[output_dataobj] = dataobj
        return True

    def finalize(self):
        self.LogInfo("finalized")
        return True

```
