long_text = """
Variopartner SICAV
529900LPCSV88817QH61
1. TARENO GLOBAL WATER SOLUTIONS FUND
LU2001709034
LU2057889995
LU2001709547
2. TARENO FIXED INCOME FUND
LU1299722972
3. TARENO GLOBAL EQUITY FUND
LU1299721909
LU1299722113
LU1299722030
4. MIV GLOBAL MEDTECH FUND
LU0329630999
LU0329630130
"""

text = long_text.split('\n');
print(text)
name = text[1];
lei = text[2];

a = long_text.count('.')
a1 = long_text.find('.')
a2 = long_text.find('FUND')
a5 = a1;
a6 = a2;

# LU数组
c = long_text.find('LU')
c1 = long_text.count('LU')

# 定义数组存放内容
title = []
LU = []
isin = []
sub_fund = []
sub = []
SUB = []
fu = []
# title数组

for i in range(0, a):
    title.append(i)
    title[i] = long_text[a5 + 1:a6]
    a5 = long_text.find('.', a5 + 1)
    a6 = long_text.find('FUND', a6 + 1)

# lu数组

for i in range(0, a):
    "从fund开始到下一个.结束，将这里的LU放入素组"
    a3 = a1;
    a1 = long_text.find('.', a3 + 1);
    # 从第一个“.”开始遍历，找到下一个点
    a4 = long_text.find('FUND', a3, a1)
    # 从第一个“.”开始遍历，到下一个“.”结束，找到fund的位置
    c2 = long_text.count('LU', a4, a1);
    # 从fund开始遍历，到下一个点结束，统计数字LU+数字的量
    c = long_text.find('LU', a4, a1);
    # 从从fund开始遍历，到下一个点结束，找到第一个LU+数字的位置
    for j in range(0, c2):
        c5 = c
        LU.append(j)
        LU[j] = long_text[c5:c + 12]
        c = long_text.find('LU', c5 + 1, a1)

    isin.append(i)
    isin.append(LU)

    LU = []

for i in range(0, len(isin)):
    if type(isin[i]) == int:
        sub.append(i)
    else:
        continue
print("sub", sub)
str1 = "title"
str2 = "isin"
str3 = "："
str4 = '\n'
for j in range(0, a):
    sub_fund.append(j)
    e = sub[j]
    SUB.append(j)
    SUB[j] = ["title：", title[j], '\n', "isin:", isin[e + 1]]
    print("SUB", SUB)
    sub_fund.append({str1: title[j], str2: isin[e + 1]})

for l in range(0, len(sub_fund)):
    if type(sub_fund[l]) == int:
        fu.append(l)
    else:
        continue

sub_fund = [sub_fund[i] for i in range(len(sub_fund)) if (i not in fu)]
endfiles = {
    'name': name,
    'lei': lei,
    'sub_fund': sub_fund

}
print(endfiles)

# 要求JSON格式输出

outputObject = {};

paragraphs = long_text.split('\n');
print(paragraphs);

# sub_fund -> 字典的List
# Keys:
# Title -> 字符串
# isin -> List

sub_fund = [];
sub_fund_object = {};
isin_list = [];

# Key 'name'
name = paragraphs[1];
outputObject["name"] = name;

# Key 'lei'
lei = paragraphs[2];
outputObject["lei"] = lei;

# 循环剩下段落
for i in range(3, len(paragraphs)):
    # 检测是否为TITLE行
    if (paragraphs[i].find('.') != -1):  # 是
        # 处理非空的 sub_fund
        if (len(isin_list) != 0):
            sub_fund_object["isin"] = isin_list;  # 设置List为上一个关键字
            sub_fund.append(sub_fund_object);  # Append进sub_fund列表

            sub_fund_object = {};  # 残留处理
            isin_list = [];

        # 设置字典
        sub_fund_object["title"] = paragraphs[i];
    else:  # 是isin行
        isin_list.append(paragraphs[i]);

# 由于最后一行没有处理sub_fund, 在这边再处理一次
if (len(isin_list) != 0):
    sub_fund_object["isin"] = isin_list;  # 设置List为上一个关键字
    sub_fund.append(sub_fund_object);  # Append进sub_fund列表

    sub_fund_object = {};  # 残留处理
    isin_list = [];

# sub_fund处理完成，给sub_fund送进Object
outputObject["sub_fund"] = sub_fund;

print(outputObject);

