#Python-Learning
#haha, just entertain myself

#读取数据
f = open('/Users/zhouyou/Desktop/Practice/sp.csv','r', encoding = 'utf8')
for i in f.readlines()[:10]:     #读取前十条
    print(i.split(',')[-1].split('                                '))       #每条按照 ','切分，选择分开后的第n项
f.seek(0)                        #光标回到第一行

#清洗数据
def fcm(s):
    if '条' in s:
        return(int(s.split(' ')[0]))  #按照' '切分，选择分开后的第1项
    else:
        return('缺失数据')             #没有'条'就反馈'缺失数据'

def fpr(s):
    if '￥' in s:
        return(int(s.split('￥')[1]))
    else:
        return('缺失数据')

def fcl(s):
    if len(s) == 3:
        quality = float(s [0][2:])    #质量为：s的第一项中第三个子后的内容
        envi = float(s[1][2:])
        ser = float(s[2][2:])
        return([quality,envi,ser])
    else:
        return('缺失数据')


for i in f.readlines()[:10]:                                                         #读取文件前十条
    commentlist = fcl(i.split(',')[-1].split('                                '))    #fcm 是被定义的函数，（）中代表未被函数检验的对象
    print(commentlist)

datalst = []
n = 0
f.seek(0)

for i in f.readlines()[1:10]:
    data = i.split(',')
    #print(data)
    classify = data[0]
    name = data[1]
    com_count = fcm(data[2])
    star = data[3]
    price = fpr(data[4])
    add = data[5]
    qua = fcl(data[-1].split('                                '))[0]
    env = fcl(data[-1].split('                                '))[1]
    ser = fcl(data[-1].split('                                '))[2]

  #print(name,com_count,star,price,add,qua,env,ser)

    if '缺失数据' not in [com_count,price,qua]:
        n += 1
        data_lst2 = [['classify',classify],
                     ['name',name],
                     ['comment',com_count],
                     ['star',star],
                     ['price',price],
                     ['add',add],
                     ['qua',qua],
                     ['environment',env],
                     ['service',ser]]
        #print(data_lst2)
        datalst.append(dict(data_lst2))
        print('成功读取第%i条数据'%n)
        print(dict(data_lst2))
    else:
        continue

f.close()
print(datalst)

import pickle

pic = open('/Users/zhouyou/Desktop/Practice/data.pkl/','wb')
pickle.dump(datalst,pic)
pic.close()

