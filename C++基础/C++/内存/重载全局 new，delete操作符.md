直接在源文件前面：
void * operator new(size_t size){
	return malloc(size);
}
void * operator new[] (size_t size){
	return malloc(size);
}

void * operator delete(void* phead){
	free(phead);
}
void * operator delete[] (void* phead){
	free(phead);
}

这个源文件中所有的都被重载了，但是影响比较大，一般不这么用
**全局的重载也会被局部重载覆盖掉**

