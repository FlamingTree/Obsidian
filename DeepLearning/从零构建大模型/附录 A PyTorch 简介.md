> [!PDF|note] [[从零构建大模型.pdf#page=253&selection=99,0,100,3&color=note|从零构建大模型, p.236]]
> > @运算符

> [!PDF|] [[从零构建大模型.pdf#page=256&selection=184,0,197,7|从零构建大模型, p.239]]
> > 默认情况下，PyTorch 在计算梯度后会销毁计算图以释放内存。然而，由于我们即将再次使用这个计算图，因此可以设置 retain_graph=True，使其保留在内存中
> 
> 

> [!PDF|] [[从零构建大模型.pdf#page=256&selection=142,23,153,2|从零构建大模型, p.239]]
> > 可以对损失函数调用.backward 方法，随后 PyTorch 将计算计算图中所有叶节点的梯度，这些梯度将通过张量的.grad 属性进行存储

> [!PDF|] [[从零构建大模型.pdf#page=261&selection=11,1,12,15|从零构建大模型, p.244]]
> > `torch.no_grad()`

> [!PDF|note] [[从零构建大模型.pdf#page=265&selection=72,8,73,7&color=note|从零构建大模型, p.248]]
> > 启动多个工作进程的开销可能会比实际数据加载所需的时间更长，尤其是数据集很小的时候。
> 
> 


> [!PDF|note] [[从零构建大模型.pdf#page=266&selection=74,0,74,13&color=note|从零构建大模型, p.249]]
> > `model.train()`

> [!PDF|note] [[从零构建大模型.pdf#page=266&selection=98,0,98,21&color=note|从零构建大模型, p.249]]
> > `optimizer.zero_grad()`

> [!PDF|note] [[从零构建大模型.pdf#page=266&selection=123,0,123,12&color=note|从零构建大模型, p.249]]
> > `model.eval()`

> [!PDF|note] [[从零构建大模型.pdf#page=270&selection=28,0,28,10&color=note|从零构建大模型, p.253]]
> > `torch.save`

> [!PDF|note] [[从零构建大模型.pdf#page=270&selection=63,0,63,21&color=note|从零构建大模型, p.253]]
> > `model.load_state_dict`

> [!PDF|note] [[从零构建大模型.pdf#page=272&selection=53,0,53,8&color=note|从零构建大模型, p.255]]
> > `model.to`
> 
> 

> [!PDF|] [[从零构建大模型.pdf#page=273&selection=156,1,161,1|从零构建大模型, p.256]]
> > 分布式数据并行（DistributedDataParallel，DDP）

> [!PDF] DDP 模型和数据传输
> ![[从零构建大模型.pdf#page=274&rect=74,389,484,604|从零构建大模型, p.257]]

> [!PDF] DDP 梯度计算和同步
> ![[从零构建大模型.pdf#page=274&rect=70,125,488,283|从零构建大模型, p.257]]

> [!PDF|] [[从零构建大模型.pdf#page=277&selection=34,8,51,9|从零构建大模型, p.260]]
> > 主函数有一个 rank 参数，我们在 mp.spawn() 调用中并未包含它。这是因为 rank（我们用作 GPU ID 的进程 ID）已经自动传递了。

> [!PDF|] [[从零构建大模型.pdf#page=277&selection=203,0,203,20|从零构建大模型, p.260]]
> > CUDA_VISIBLE_DEVICES
> 
> 

> [!PDF|] [[从零构建大模型.pdf#page=278&selection=416,0,418,12|从零构建大模型, p.261]]
> > Accelerating PyTorch Model Training: Using Mixed-Precision and Fully Sharded Data Parallelism
>  
> https://github.com/rasbt/cvpr2023

