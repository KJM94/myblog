---
title: Bubble_sort
date: 2021-06-06 01:44:10
tags: Java
---

# 버블 정렬

- 무작위의 정수값을 배열에 넣은 후 오름차순 정렬

```
package test1;

import java.util.Scanner;
import java.util.List;
import java.util.ArrayList;
import java.util.Collections;

public class prac {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		int inum;  //  입력받을 배열 수
		int i;
		int nnum;  //  배열에 들어갈 숫자
		
		List nlist = new ArrayList();
		
		Scanner input = new Scanner(System.in);
		
		System.out.print("배열 수 입력 : ");
		inum = input.nextInt();
		
		for(i = 0; i < inum; i++) {
			System.out.print("배열에 들어갈 숫자 : ");
			nnum = input.nextInt();
			nlist.add(nnum);
		}
		
		System.out.print("정렬 전 배열 : ");
		System.out.println(nlist);
		
		Collections.sort(nlist);  //  오름차순 정렬
		
		System.out.print("정렬 후 배열 : ");
		System.out.println(nlist);
		
		
	}

}
```
