---
tags: []
title:
dg-publish: true
created: 2023-09-20 10:05
updated: 2025-06-10 15:00
---
```python
import typer


def compare_lists(list1, list2):
    total = 0
    same = 0
    if len(list1) != len(list2):
        return False
    for item1, item2 in zip(list1, list2):
        total += 1
        if item1 == '00' or item2 == '00':   # what is the symbol of NA in ped?
            continue
        else:
            if item1 == item2:
                same +=1
    return [same, total]

def compare_genotypes(ped: typer.FileText = typer.Option(..., '--ped', '-p', help='包含所有待比较样本的PED文件'),
					  lst: typer.FileText = typer.Option(..., '--lst', '-l', help='指定待比较样本的LIST文件，格式为每行一对待比较样本名，以空格或制表符分割'),
					  #out: typer.FileTextWrite  = typer.Option(..., '--out', '-o', help='输出文件名')):  这样的话参数非必须
					  out: typer.FileTextWrite  = typer.Option(..., '--out', '-o', help='输出文件名')):
	'''
	比较指定样本之间的基因型一致率

	--ped: 标准PLINK格式的PED文件

	--lst: 指定样本名，例如：

	\n
	\tALT1  ALT2

	\tALT2  ALT3

	'''
	iid_gt_dct = {}
	for line in ped:
		lines = line.strip().split()
		iid = lines[1]
		gt = [lines[6:][i:i+2] for i in range(0, len(lines), 2)]
		iid_gt_dct[iid] = gt

	for line in lst:
		lines = line.strip().split()
		id1, id2 = lines[0], lines[1]
		gt1, gt2 = iid_gt_dct.get(id1, 'NA'), iid_gt_dct.get(id2, 'NA')
		num_lst = compare_lists(gt1, gt2)
		out.write('\t'.join([id1, id2, str(num_lst[0]), str(num_lst[1]), str(round(num_lst[0]/num_lst[1], 4))]) + '\n')

if __name__ == '__main__':
	typer.run(compare_genotypes)
```