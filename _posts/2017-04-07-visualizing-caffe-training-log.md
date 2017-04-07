---
layout: post
title: "Visualizing Caffe training log"
date: 2017-04-07 
description: training log chart.
tags:
 - CaffeTools
 - DeepLearning
---

# Visualizing Caffe training log

## Revision

This tool modify the plot_training_log.py from caffe extra tools due to the inconvenient implement for .sh file in windows. After changing some functions, this python code can be excuted without .sh file because of that a parse_log.py can extract train and test data.

### Revising [plot_training_log.py](https://github.com/xiaopanlyu/caffe/blob/master/tools/plot_training_log.py).
Two revisions in [plot_training_log.py](https://github.com/xiaopanlyu/caffe/blob/master/tools/plot_training_log.py) as following.

* Revising plot_chart

```python
import parse_log

......

def plot_chart(chart_type, path_to_png, path_to_log_list):
    for path_to_log in path_to_log_list:
        #print "path_to_log %s" % path_to_log
        #comment this function
        #os.system('%s %s' % (get_log_parsing_script(), path_to_log))
        
        #extract train and test data respectively.==========================================
        train_dict_list, test_dict_list = parse_log.parse_log(path_to_log)
        (filepath, log_basename) = os.path.split(path_to_log)
        #print('filepath' + filepath)
        parse_log.save_csv_files(path_to_log, filepath+'/', train_dict_list,test_dict_list)
        #====================================================================

        data_file =filepath+'/'+ get_data_file(chart_type, path_to_log)
        
        ......
      
    #plt.show()
```

* Revising field index.
```python
def create_field_index():
    train_key = 'Train'
    test_key = 'Test'
    
    # here index should be corresponding to the parse log data=============
    field_index = {train_key:{'Iters':0, 'Seconds':1, train_key + ' learning rate':2,
                              train_key + ' loss':3},
                   test_key: {'Iters':0, 'Seconds':1, test_key + ' accuracy':3,
                              test_key + ' loss':4}}
    #======================================================================
    
    fields = set()
    #print len(field_index.keys())
    for data_file_type in field_index.keys():
        fields = fields.union(set(field_index[data_file_type].keys()))
    fields = list(fields)
    fields.sort()
    #print fields
    return field_index, fields
```
## Usage
Here, we write a [bat script](https://github.com/xiaopanlyu/caffe/blob/master/tools/plot_training_log.bat) so that it's easily to use. For convenience, user can just run the bat file in windows.

## Result Chart

Here, we can get 8 different charts by the  field index union.

- Test accuracy vs. Iters
- Test accuracy vs. Seconds
- Test loss vs. Iters
- Test loss vs. Seconds
- Train learning rate vs. Iters
- Train learning rate vs. Seconds
- Train loss  vs. Iters
- Train loss vs. Seconds

![img]({{ site.baseurl | prepend:site.url}}/images/Log-LogReg_solver-1.log_0.png){: .center-image }*(Test accuracy vs. Iters)*
![img]({{ site.baseurl | prepend:site.url}}/images/Log-LogReg_solver-1.log_1.png){: .center-image }*(Test accuracy vs. Seconds)*
![img]({{ site.baseurl | prepend:site.url}}/images/Log-LogReg_solver-1.log_2.png){: .center-image }*(Test loss vs. Iters)*
![img]({{ site.baseurl | prepend:site.url}}/images/Log-LogReg_solver-1.log_3.png){: .center-image }*(Test loss vs. Seconds)*
![img]({{ site.baseurl | prepend:site.url}}/images/Log-LogReg_solver-1.log_4.png){: .center-image }*(Train learning rate vs. Iters)*
![img]({{ site.baseurl | prepend:site.url}}/images/Log-LogReg_solver-1.log_5.png){: .center-image }*(Train learning rate vs. Seconds)*
![img]({{ site.baseurl | prepend:site.url}}/images/Log-LogReg_solver-1.log_6.png){: .center-image }*(Train loss  vs. Iters)*
![img]({{ site.baseurl | prepend:site.url}}/images/Log-LogReg_solver-1.log_7.png){: .center-image }*(Train loss vs. Seconds)*

## Reference
- Code:[https://github.com/xiaopanlyu/caffe/tree/master/tools](https://github.com/xiaopanlyu/caffe/tree/master/tools)

