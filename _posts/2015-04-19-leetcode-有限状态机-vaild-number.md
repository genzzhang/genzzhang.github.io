---
layout: post
title: leetcode:【有限状态机】Valid Number    
date: 2015-04-19 18:35
categories: 算法
tags: leetcode
---

# 问题描述 Valid Number  

Validate if a given string is numeric.

>Some examples:  
"0" => true  
" 0.1 " => true  
"abc" => false  
"1 a" => false  
"2e10" => true  

# code

```
/*
note1:123.E3 false == 123.e3 true 
https://leetcodenotes.wordpress.com/2013/11/23/leetcode-valid-number/
这就是典型的try and fail，尼玛谁知道什么算是数什么不算啊？让我用眼睛看我也不知道啊！总之，试出来的规则是这样的：
AeB代表A * 10 ^ B
A可以是小数也可以是整数，可以带正负号
.35, 00.神马的都算valid小数；就”.”单独一个不算
B必须是整数，可以带正负号
有e的话，A,B就必须同时存在
算法就是按e把字符串split了，前面按A的法则做，后面按B做。
感觉hasdot很nice，题目有点问题吧
*/
public class Solution {
    public boolean isNumber(String s) {
        //" 3e "
        s = s.trim(); 
        if(s.length() == 0 || s.charAt(s.length()-1) == 'e') return false;
        String str[] = s.split("e");
        if(str.length == 0 || str.length>2) return false;
        boolean res = vaildNum(str[0],false);
        if(str.length>1) {
            res = res && vaildNum(str[1],true);
        }
        return res;
    }
    private boolean vaildNum(String s,boolean hasDot) {
        if(s.length() > 0 && (s.charAt(0) == '+' || s.charAt(0) == '-'))
            s = s.substring(1);
        char[] arr = s.toCharArray();
        //".."   
        if(arr.length == 0 || s.equals(".")) return false;
        for(int i = 0;i<arr.length;i++) {
            if(arr[i] == '.') {
                if(hasDot) return false;
                hasDot = true;
            } else if(arr[i] < '0' || arr[i] > '9') {
                return false;
            }
        }
        return true;
    }
}



/*
有限状态机FSM：【当前状态+输入】->【动作：下一状态】
应用：数字电子逻辑设计、正则表达式、游戏设计、自动客服、词法分析、字符串匹配等
有限状态自动机拥有有限数量的状态，每个状态可以迁移到零个或多个状态，输入决定执行哪个状态的迁移。
有限状态自动机可以表示为一个有向图。
*/
/*********输入类型*************/	
	INVAILD,//无效
	SPACE, //空格
	SIGN,//+ -
	DIGIT,//数字
	DOT,//点
	EXPONENT,//E e	
初始态为0，无效状态为-1.
0态后接着6种输入对应5种状态，接着在这5种状态下各自对应着6种输入得出新的状态，如此下去分析。
/*********状态分析*************/	
起始为0：
　　当输入空格时，状态仍为0，
　　输入为符号时，状态转为3，3的转换和0是一样的，除了不能再接受符号，故在0的状态的基础上，把接受符号置为-1；
　　当输入为数字时，状态转为1, 状态1的转换在于无法再接受符号，可以接受空格，数字，点，指数；状态1为合法的结束状态；
　　当输入为点时，状态转为2，状态2必须再接受数字，接受其他均为非法；
　　当输入为指数时，非法；

状态1：
　　接受数字时仍转为状态1，
　　接受点时，转为状态4，可以接受空格，数字，指数，状态4为合法的结束状态，
　　接受指数时，转为状态5，可以接受符号，数字，不能再接受点，因为指数必须为整数，而且必须再接受数字；

状态2：
　　接受数字转为状态4；

状态3：
　　和0一样，只是不能接受符号；

状态4：
　　接受空格，合法接受；
　　接受数字，仍为状态4；
　　接受指数，转为状态5，

状态5：
　　接受符号，转为状态6，状态6和状态5一样，只是不能再接受符号，
　　接受数字，转为状态7，状态7只能接受空格或数字；状态7为合法的结束状态；

状态6：
　　只能接受数字，转为状态7；

状态7：
　　接受空格，转为状态8，状态7为合法的结束状态；
　　接受数字，仍为状态7；

状态8：
　　接受空格，转为状态8，状态8为合法的结束状态；
/*********code*************/	
class Solution {
public:
	bool isNumber(const char *s) {
		enum InputType {
			INVALID,		// 0 Include: Alphas, '(', '&' ans so on
			SPACE,		// 1
			SIGN,		// 2 '+','-'
			DIGIT,		// 3 numbers
			DOT,			// 4 '.'
			EXPONENT,		// 5 'e' 'E'
		};
		int transTable[][6] = {
		//0INVA,1SPA,2SIG,3DI,4DO,5E
			-1,  0,  3,  1,  2, -1,//0初始无输入或者只有space的状态
			-1,  8, -1,  1,  4,  5,//1输入了数字之后的状态
			-1, -1, -1,  4, -1, -1,//2前面无数字，只输入了Dot的状态
			-1, -1, -1,  1,  2, -1,//3输入了符号状态
			-1,  8, -1,  4, -1,  5,//4前面有数字和有dot的状态
			-1, -1,  6,  7, -1, -1,//5'e' or 'E'输入后的状态
			-1, -1, -1,  7, -1, -1,//6输入e之后输入Sign的状态
			-1,  8, -1,  7, -1, -1,//7输入e后输入数字的状态
			-1,  8, -1, -1, -1, -1,//8前面有有效数输入之后，输入space的状态
		};
		int state = 0;
		while (*s)
		{
			InputType input = INVALID;
			if (*s == ' ') input = SPACE;
			else if (*s == '+' || *s == '-') input = SIGN;
			else if (isdigit(*s)) input = DIGIT;
			else if (*s == '.') input = DOT;
			else if (*s == 'e' || *s == 'E') input = EXPONENT;
			state = transTable[state][input];
			if (state == -1) return false;
			++s;
		}
		return state == 1 || state == 4 || state == 7 || state == 8;
	}
};

/*********感想*************/
状态设计脑子晕乎乎的，上面的直接是copy人家的，没有特别的去想，就当做是一种记录。
状态很明显的就比较好eg:输入串为3进制数，输出为模5的余数.
模5的余数总共只有5个（0，1，2，3，4）这就是5个状态。初始状态为0。
三进制每位数有3种可能，因此每种状态有3种跃迁可能。
把3进制串理解成从高位到低位一个一个输入，每条输入就是一次跃迁
状态就是到当前输入为止的3进制数模5的余数。
跃迁的函数如下：
目标状态 = （当前状态 *进制数 + 串的当前位）% 5。
举例如下：
三进制数 12112
当前状态 输入 跃迁
0(start) 1 (0*3+1) % 5 = 1
1 2 (1*3+2) % 5 = 0
0 1 (0*3+1) % 5 = 1
1 1 (1*3+1) % 5 = 4
4 2 (4*3+2) % 5 = 4 (最终结果)
```