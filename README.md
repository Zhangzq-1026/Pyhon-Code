# Pyhon-Code
自学python笔记

# 带着二哈去旅行丶:ZZQ
# 生成时间：2021/8/9 15:41
import os
filename = 'student.txt'      # 定义文件名
filename1 = 'student1.txt'
# 2.主函数
def main():
    menu()
    while True:
        choice = int(input('请选择功能（0~7）：'))  # global+局部变量=全局变量
        if choice in [0, 1, 2, 3, 4, 5, 6, 7]:
            if choice == 0:
                answer = input('您确定要退出系统嘛？（y/n）：')
                if answer == 'y' or answer == 'Y':
                    print('谢谢您的使用！！')
                    break
                else:
                    continue
            elif choice == 1:
                insert()
            elif choice == 2:
                search()
            elif choice == 3:
                delete()
            elif choice == 4:
                modify()
            elif choice == 5:
                sort()
            elif choice == 6:
                total()
            elif choice == 7:
                show()
        else:
            print('您的输入有错，请重新输入！')
        answer = input('是否继续使用该系统？（y/n）：')
        if answer == 'y' or answer=='Y':
            menu()
        else:
            break
# 1.先写菜单函数
def menu():
    print('======================学生信息管理系统==========================')
    print('-------------------------功能菜单------------------------------')
    print('\t\t\t\t1.录入学生信息')
    print('\t\t\t\t2.查找学生信息')
    print('\t\t\t\t3.删除学生信息')
    print('\t\t\t\t4.修改学生信息')
    print('\t\t\t\t5.排序')
    print('\t\t\t\t6.统计学生人数')
    print('\t\t\t\t7.显示所有学生信息')
    print('\t\t\t\t0.退出')
    print('--------------------------------------------------------------')
# 3.定义功能函数:
# 4.录入
def insert():       # 录入信息
    student_list=[]
    while True:         # 循环录入
        id=input('请输入ID（1001）：')
        if not id:      # 空的布尔值为F not id 为T
            break
        name=input('请输入姓名：')
        if not name:
            break
        try:
            English=int(input('请输入English成绩：'))
            Python=int(input('请输入Python成绩：'))
            Java=int(input('请输入Java成绩：'))
        except:
            print('输入无效，不是整数类型，请重新输入')
            continue
        # 将录入的学生信息保存到字典中
        student={'id':id,'name':name,'English':English,'Python':Python,'Java':Java}
        student_list.append(student)    # 将学生信息添加到学生列表
        answer=input('是否继续添加？（y/n）：')
        if answer=='y' or answer=='Y':
            continue
        else:
            break

    # 调用save()函数保存学生信息
    save(student_list)
    save_sort_ID()
    print('学生信息录入完毕！')
# 5.创建文件保存
def save(lst):
    try:
        stu_txt=open(filename,'a',encoding='utf-8')     # 如果文件存在，则以追加方式添加
    except:
        stu_txt=open(filename,'w',encoding='utf-8')     # 只写模式打开文件，文件不存在，则创建一个文件
    for item in lst:
        stu_txt.write(str(item)+'\n')
    stu_txt.close()
# 保存到另外文件中
def save1(lst1):
    # try:
    #     stu_txt = open(filename1, 'a', encoding='utf-8')  # 如果文件存在，则以追加方式添加
    # except:
    stu_txt = open(filename1, 'w', encoding='utf-8')  # 只写模式打开文件，文件不存在，则创建一个文件，该文件是复制，不需要追加
    for item in lst1:
        stu_txt.write(str(item) + '\n')
    stu_txt.close()
# 以学号排序并另存
def save_sort_ID():
    student_new = []  # 存放读取的字典的列表
    if os.path.exists(filename):  # 判断文件是否存在，存在则进行拷贝以学号排序保存
        with open(filename, 'r', encoding='utf-8') as rfile:
            students = rfile.readlines()
        for item in students:
            student_new.append(eval(item))
    else:
        print('暂未保存数据！')
    student_new.sort(key=lambda student_new: int(student_new['id']), reverse=False)  # 以学号排序
    save1(student_new)
# 8.查询
def search():
    student_query=[]
    while True:
        id=''
        name=''
        if os.path.exists(filename):
            mode=input('按ID查找请输入1，按姓名查找请输入2：')
            if mode=='1':
                id=input('请输入学生ID：')
            elif mode=='2':
                name=input('请输入学生姓名：')
            else:
                print('您的输入有误，请重新输入！')
                search()
            with open(filename,'r',encoding='utf-8') as rfile:
                student=rfile.readlines()   # 只读打开文件，存到student列表中，每行读取
                for item in student:        # for遍历列表，得到每行信息
                    d=dict(eval(item))      # 将列表每行信息转换成字典
                    if id!='':
                        if d['id']==id:
                            print('查找到该学生：')
                            student_query.append(d)
                            show_title()
                    elif name!='':
                        if d['name']==name:
                            print('查找到该学生：')
                            student_query.append(d)
                            show_title()
            # 显示查询结果
            show_student(student_query)
            # 清空列表，防止下次查询还出现
            student_query.clear()
            answer = input('是否需要继续查询？（y/n）：')
            if answer == 'y' or answer == 'Y':
                print('请继续查询学生信息：')
                continue
            else:
                print('请继续其他操作：')
                break
        else:
            print('暂未保存数据！')
            break
# 9.打印查询信息列表
# 打印标题
def show_title():
    # 定义标题显示格式
    format_title = '{:^6}\t{:^12}\t{:^8}\t{:^8}\t{:^8}\t{:^8}'
    print('-------------------------------------------------------------------')
    print(format_title.format('ID', '姓名', 'English成绩', 'Python成绩', 'Java成绩', '总成绩'))
# 打印内容
def show_student(lst):
    if len(lst)==0:
        print('没有查询到学生信息，无数据显示！！')
        return
    # 定义内容显示格式
    format_data='{:^6}\t{:^12}\t{:^8}\t{:^8}\t{:^8}\t{:^8}'
    for item in lst:
        print(format_data.format(item.get('id'),
                                 item.get('name'),
                                 item.get('English'),
                                 item.get('Python'),
                                 item.get('Java'),
                                 int(item.get('English'))+int(item.get('Python'))+int(item.get('Java'))
                                 ))
# 6.删除
def delete():
    while True:     # 控制循环
        student_id=input('请输入要删除的学生ID：')
        if student_id!='':
            if os.path.exists(filename):        # 判断文件是否存在
                # 上下文文件管理器打开文件
                with open(filename,'r',encoding='utf-8') as file:
                    student_old=file.readlines()    # 读取所有数据存到另一个列表中
            else:
                student_old=[]
            flag=False      # 标记是否删除
            if student_old:     # 文件存在，将原有文件进行覆盖
                with open(filename,'w',encoding='utf-8') as wfile:
                    d={}
                    for item in student_old:
                        d=dict(eval(item))      # 将字符串转换成字典存放
                        if d['id']!=student_id:   # 与学生信息不相等 写入文件
                            wfile.write(str(d)+'\n')
                        else:
                            flag=True       # 相等 表示删除
                    if flag:
                        print(f'id为{student_id}的学生信息已经被删除')
                    else:
                        print(f'没有找到id为{student_id}的学生信息')
            else:
                print('无此学生信息')
                break
      #     删除操作有问题
            show()          # 删除之后重新显示所有学生信息
            answer=input('是否继续删除？（y/n）：\n')
            if answer=='y' or answer=='Y':
                continue
            else:
                break
# 7.修改
def modify():
    show()
    if os.path.exists(filename):    # 判断文件是否存在
        with open(filename,'r',encoding='utf-8')as rfile:  # 存在则创建一个文件保存内容到另一个列表中
            student_old=rfile.readlines()
    else:
        return
    # student_id = input('请输入要修改的学员ID：')
    with open(filename,'w',encoding='utf-8') as wfile:  # 创建文件
        student_id = input('请输入要修改的学员ID：')
        for item in student_old:
             d=dict(eval(item))      # 列表转换为字典存放
        #  人是否存在判定
             if d['id']==student_id:
                print('找到学生信息，可以修改相关信息！')
                while True:
                    try:
                        d['name']=input('请输入姓名：')
                        d['English']=int(input('请输入英语成绩：'))
                        d['Python']=int(input('请输入Python成绩：'))
                        d['Java']=int(input('请输入Java成绩：'))
                    except:
                        print('您输入的信息有误，请重新输入！！')
                    else:
                        break
                wfile.write(str(d) + '\n')  # 将修改信息写入文件
                print('修改成功！')
                #   问题：如果没查到这个人，数据会被清空
             else:
                print('学号输入错误，请重新输入！')
                wfile.write(str(d)+'\n')        # 将未修改信息写入文件
    show()
    answer=input('是否需要继续修改其他学生信息？（y/n）：')
    if answer=='y' or answer=='Y':
         print('请继续修改学生信息：')
         modify()
    else:
         print('请继续其他操作：')
         return
# 12.排序
def sort():
    show()
    student_new=[]      # 存放读取的字典的列表
    if os.path.exists(filename1):
        with open(filename1,'r',encoding='utf-8') as rfile:
            students=rfile.readlines()
        for item in students:
            student_new.append(eval(item))
    else:
        print('暂未保存数据！')
    asc_or_desc=input('请选择（0：升序，1：降序）：')
    if asc_or_desc=='0':
        asc_or_desc_bool=False          # F为升序
    elif asc_or_desc=='1':
        asc_or_desc_bool=True           # T为降序
    else:
        print('您的输入有误，请重新输入！')
        sort()
    mode=input('请选择排序方式：1.按English排序 2.按Python排序 3.按Java排序 0.按总成绩排序 ：')
    if mode=='1':
        student_new.sort(key=lambda student_new:int(student_new['English']),reverse=asc_or_desc_bool)      # key排序项 后面student_new指列表中的每一项可以更换字符 reverse排序方式
    elif mode=='2':
        student_new.sort(key=lambda student_new: int(student_new['Python']), reverse=asc_or_desc_bool)
    elif mode=='3':
        student_new.sort(key=lambda student_new: int(student_new['Java']), reverse=asc_or_desc_bool)
    elif mode=='0':
        student_new.sort(key=lambda student_new: int(student_new['English'])+int(student_new['Python'])+int(student_new['Java']), reverse=asc_or_desc_bool)
    else:
        print('您的输入有误，请重新输入！')
        sort()
    show_student(student_new)
    answer = input('是否继续选择其他排序方式？（y/n）：')
    if answer == 'y' or answer == 'Y':
        sort()
    else:
        return
# 10.统计
def total():
    if os.path.exists(filename1):
        with open(filename1,'r',encoding='utf-8') as rfile:
            students=rfile.readlines()      # 读取文件存到列表
            if students:
                print(f'总共有{len(students)}名学生信息。')
            else:
                print('暂无学生信息。')
    else:
        print('暂未保存数据！')
# 11.打印
def show():
    student_lst=[]
    save_sort_ID()      # 每次显示之前复制filename内容并排序
    if os.path.exists(filename1):  # 判断文件是否存在
        with open(filename1,'r',encoding='utf-8') as rfile:
            students=rfile.readlines()      # 存放
        for item in students:               # 遍历 转换为字典输出
            student_lst.append(eval(item))
        if student_lst:
            show_title()                    # 打印标题
            show_student(student_lst)       # 不为空打印出来
        else:
            show_title()
    else:
        print('暂未保存数据！')
# 以主程序的方式运行
if __name__=='__main__':
    main()
