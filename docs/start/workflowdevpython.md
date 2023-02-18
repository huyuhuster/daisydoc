# 工作流实现方法

&emsp;&emsp;工作流一般由线站科学家实现。工作流和算法一样，也必须实现 initialize、execute和 finalize三个方法，对应工作流初始化、执行和销毁三个阶段。具体实现参考下面是 CT 图像重建的工作流。

```python
#!/usr/bin/env python
# -*- coding:utf-8 -*-

# using the new Workflow Engine for data analysis.

import Daisy 
#  The initialization parameters list for the algorithm objects
init_dict   = {
                'loadtomo':{'class_name':'LoadTIFs',},\
                'loaddark':{'class_name':'LoadTIFs',},\
                'loadflat':{'class_name':'LoadTIFs',},\
                'loadreco':{'class_name':'LoadHDF5',},\
             'filterdata':{'class_name':'AlgSelectSinogram',},\
              'normalize':{'class_name':'AlgTomopyNormalize',},\
                'angles':{'class_name':'AlgTomopyAngles',},\
                'minuslog':{'class_name':'AlgTomopyMinuslog',},\
             'reconstruct':{'class_name':'AlgTomopyRecon',},\
               'savedata':{'class_name':'SaveHDF5',\
                          'init_paras':{'outputfile_name':'tomopy_scan.h5'}\
                          }\
              }

# The Algorithm running parameter list
cfg_dict    = {
              'loadtomo':{'input_path':'/opt/CT/ZY-2/'},
              'loaddark':{'input_path':'/opt/CT/ZY-2/'},
              'loadflat':{'input_path':'/opt/CT/ZY-2/'},
              'loadreco':{'inputfile_name':'/opt/CT/ZY-2/tomopy_reco.h5'}
}


@Daisy.Base.Singleton
class WorkflowCTReconstruct(Daisy.Base.PyWorkflow):
    # Implementation of the methodological  logic that invokes algorithms or subworkflows sequentially
    def execute(self):
        self.engine['loadtomo'].execute(startswith='tomo_',seperator='_', idx=2, output_dataobj='tomodata')
        self.engine['loaddark'].execute(startswith='dark_',seperator='_', idx=1, output_dataobj='darkdata')
        self.engine['loadflat'].execute(startswith='flat_',seperator='_', idx=1, output_dataobj='flatdata')
        self.engine['normalize'].execute(projs_dataobj='tomodata', darks_dataobj='darkdata', flats_dataobj='flatdata', output_dataobj='normdata')
        self.engine['angles'].execute(input_dataobj='normdata', output_dataobj='thetas')
        self.engine['minuslog'].execute(input_dataobj='normdata', output_dataobj='mlogdata')
        self.engine['reconstruct'].execute(input_dataobj='mlogdata', theta='thetas',center=1030, alg_type='fbp', output_dataobj='recodata')
        self.engine['savedata'].execute(input_dataobj='recodata', output_path='/entry/reco' )

if __name__ == "__main__":
    wf = WorkflowCTReconstruct('WorkflowCTReconstruct')
    wf.setLogLevel(5)
    # Workflow engine needs to be specified during workflow initialization
    # The list of algorithm object initialization parameters and algorithm run parameters can be represented by a JSON object or a Python dictionary object
    wf.initialize(workflow_engine='PyWorkflowEngine', workflow_environment = init_dict, algorithms_cfg = cfg_dict)
    
    # Execute workflow
    wf.execute()
    # Get the list of algorithm object
    #algs =wf.algorithm_keys()
    # Get the algorithm object
    #wf.get_algorithm('loadmask')
    # Get the list of data object
    #data =wf.data_keys()
    # Get the data object
    #wf.get_data('result'))

    wf.finalize()

```

&emsp;&emsp;其中 self.engine 和 self.engine.datastore 为初始化时导入的工作流引擎以及对应的数据仓库。在工作流执行过程中，不同的工作流引擎和数据仓库对应不同的计算环境； self.engine 和 self.engine.datastore 分别维护一组 key-value字典，可以通过算法名和数据对象名，访问算法对象和数据对象。
