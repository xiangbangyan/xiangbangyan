# 作业一：批量删除文件名中的字符
# import os

# # 设置要处理的文件夹路径
# folder = '/home/xiangbangyan/workspace/mine/20230504'
# # 要删除的字符
# char_to_remove = '00_'
# # 遍历文件夹下所有文件和目录
# for root, dirs, files in os.walk(folder):
#   # 遍历所有文件
#   for filename in files:
#     # 如果文件名包含要删除的字符
#     if char_to_remove in filename:
#       # 删除该字符
#       new_filename = filename.replace(char_to_remove, '')
#       old_path = os.path.join(root, filename)
#       new_path = os.path.join(root, new_filename)      
#       # 重命名文件
#       os.rename(old_path, new_path)
# print('完成！')


#--------------------------------------------------------------------------------------------------------------------------

# 作业二：将所有文件路径写入一个txt文件
# -- coding: utf-8 --
# import os
# folder = '/home/xiangbangyan/workspace/mine/homework/20230711/'
# with open('paths.txt', 'w') as f:
#   for root, dirs, files in os.walk(folder):
#     for filename in files:
#       # 拼接完整的文件路径 
#       fullpath = os.path.join(root, filename)
#       # 写入文件
#       f.write(fullpath + '\n')
# # 这里的txt文件会写到运行环境的路径之下
# print('完成!')

#-------------------------------------------------------------------------------------------------------------------------
# #作业三：txt文件的查找替换
# import os
# import re
# with open('paths.txt','r+') as f:
#   content = f.read()
#   content = content.replace('/', '_')
#   f.seek(0)
#   f.write(content)
#   f.truncate() 
# print('完成！')

# # 用正则去除开头的/
# import re

# with open('paths.txt', 'r+') as f:
#   lines = f.readlines()

#   new_lines = []
#   for line in lines:
#     new_line = re.sub(r'^/+', '', line) 
#     new_lines.append(new_line)

#   f.seek(0)  
#   f.writelines(new_lines)
#   f.truncate()
# print('完成！')

#-----------------------------------------------------------------------------------------------------------------------------------
#作业四：交换文件层级
import os
import shutil

# 读取变量的值
# path_current表示需要交换层级所在的任意一个完整的绝对路径
# any_folder_name_parent表示path_current中的要交换的父文件夹名
# any_folder_name_child表示path_current中的要交换的子文件夹名
path_current = '/home/xiangbangyan/workspace/mine/homework/20230711/1.98mm/sit/person001/15lux/clean/c14_0001.bin'
folder_name_parent = '1.98mm'
folder_name_child = 'person001'

# 从右向左分割路径；head_parent表示要交换层级的父文件夹的上一层路径
head_parent, tail_parent = os.path.split(path_current)
while tail_parent != folder_name_parent:
    head_parent, tail_parent = os.path.split(head_parent)
print(head_parent) # = /home/xiangbangyan/workspace/mine/homework/20230711/1.98mm/sit
#print(tail_parent) # = person001

# items_parent表示要交换层架的父文件夹中的所有文件夹
items_parent = os.listdir(head_parent)
print(items_parent) # =['sit', 'stand',]

# items_child表示要交换层架的子文件夹中的所有文件夹
head_child = os.path.join(head_parent, folder_name_parent)
items_child = os.listdir(head_child)
print(items_child) # =['person001', 'person001', 'person002'......]

# 新建新的文件夹路径，将旧的文件夹下的所有文件移动到新的文件夹下
for i_parent in items_parent:
    for i_child in items_child:
        old_path = os.path.join(head_parent, i_parent, i_child)
        new_path = os.path.join(head_parent, i_child, i_parent)
        if not os.path.exists(new_path):
            os.makedirs(new_path)
            files = os.listdir(old_path)
            for file in files:
                old_file = os.path.join(old_path,file)
                new_file = os.path.join(new_path,file)
                shutil.move(old_file, new_file)

# 删除旧的文件夹及其文件
for i_parent in items_parent:
    to_delete_path = os.path.join(head_parent, i_parent)
    shutil.rmtree(to_delete_path)
print('完成！')
#------------------------------------------------------------------------------------------------------------------------------


