# printf()


# puts()
> puts 是 output string 的缩写，只能用来输出字符串


**使用 `puts()` 函数输出长文本**
使用puts输出长文本时可以选择多写几个puts() 函数
```c
#include <stdio.h>

int main(){
	puts("最是人间留不住 朱颜辞镜花辞树");
	puts("醉后不知天在水 满船清梦压星河");
	puts("山中何事？松花酿酒，春水煎茶");
	puts("驰隙流年， 恍如一瞬星霜换");
	
	return 0;
}
```

**使用 `puts()` 函数接收多个参数**
```c
#include <stdio.h>

int main(){
	puts(
		"最是人间留不住 朱颜辞镜花辞树",
		"醉后不知天在水 满船清梦压星河",
		"山中何事？松花酿酒，春水煎茶",
		"驰隙流年， 恍如一瞬星霜换"
		);
	
	return 0;
}
```
在 puts() 函数中，可以将一个较长的字符串分割成几个较短的字符串，这样会使得长文本的格式更加整齐。 但是，<mark class="hltr-blue">这只是形式上的分割，编译器在编译阶段会将它们合并为一个字符串，它们放在一块连续的内存中。而且也不是必须换行</mark>
```c
#include <stdio.h>

int main(){
	puts("莫道春来便归去", "江南虽好是他乡");
	
	return 0;
}
```
