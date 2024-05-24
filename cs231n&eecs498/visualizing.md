![[Pasted image 20240313101217.png]]![[Pasted image 20240313101308.png]]
使得整个网络架构的表征可视化，降低高斯化。
#feature_inversion
![[Pasted image 20240313101438.png]]
使得某层的特征看起来更自然一些，增强平滑。此处使用l2 loss，和l+norm
![[Pasted image 20240313101623.png]]
此处反向传播时将梯度作为下一层的激活值，相当于截断长距离的梯度传播，更加关注我们希望模型关注的特定特征或层。