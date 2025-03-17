# 高性能计算
## 提升方法
### 多线程
| 项 | 说明 | 适用场景 | 代码示例 |
| - | - | - | - |
| 标准库线程 | / | 简单直接 | std::thread |
| OpenMP | 默认使用系统的所有可用CPU核来并行执行。需编译器支持 | 简单的并行循环 |  #pragma omp parallel for |
| TBB（Intel Threading Building Blocks） | 高级并行任务调度 | 复杂的并行任务，C++推荐使用 | tbb::parallel_for |

### 指令集
| 分类 | 项 | 并行计算架构 | 说明 |
| - | - | - | - |
| CPU | AVX2 | SIMD（单指令多数据） | 一个指令同时操作多个数据元素。AVX2的寄存器的宽度是256位，一次能处理8个32位浮点数；而普通指令1次只处理1个。|
| GPU | CUDA | SIMT（单指令多线程） | 一个指令同时操作多个线程 |

## 第三方库
* 并行计算
    * [OpenCV的GPU加速](mp/OpenCV/README)
    * [Cuda](https://ai.wangyaqi.cn/#/cuda/SUMMARY)
    * [C++多线程](https://cpp.wangyaqi.cn/#/kb/mp)
    * [OpenMP并行编程](mp/OpenMP/README)
* *深度学习部署
    * [Triton](ttps://ai.wangyaqi.cn/#/triton/README)
    * [TensorRT](https://ai.wangyaqi.cn/#/trt/README)
* 图像处理
    * [itk多线程加速](https://cpp.wangyaqi.cn/#/lib/graphics/itk/mt)