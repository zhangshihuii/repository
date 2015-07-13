clear all
clc
n=63
k=45
d=41
error_rate=0.001
numbers=10


NN=200;%%%%NN*n>p*q
message=randint(NN,k);
%%%%%%% %%%%%%%%%%%
%%%%%%%%%%%%%对序列进行编码
[pg, pm, cs, hh, tt]=bchpoly(n, k);%%%生成多项式  
g=g2G(k,fliplr(pg));%%%%%根据生成多项式产生标准生成矩阵
encoded_data1=mod(message*g,2);%%%%%信息直接与生成矩阵相乘进行编码
encoded_data=reshape(encoded_data1',1,n*NN);%%%对编码后的序列进行降维
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%



%%%%%%%%%%%加入错误码字
errorrate=error_rate;%~~~~~~~误码率
a=randperm(NN*n);
b=a(1:errorrate*n*NN);
for i=1:length(b)
    if encoded_data(b(i))==1
        encoded_data(b(i))=0;
    else
        encoded_data(b(i))=1;
    end
end
% %%%%%%%%%%%%%%%%%%%




%%%%%%%%%%%%%%%%%
delay=n-d;%%%%%%%延时
rec_data=encoded_data(delay+1:end);%%%%%%接收到的数据


%%%%%%%%%%%%%%%%%%%%%%码长估计
 f1=[];
 l1=[];
 p1=150;%%%%%%%行数尽可能的大于列数
     q1=n;
 for syc=1:n
     syc;
a1=rec_data(1,1+syc:((p1*q1)+syc));
b1=reshape(a1,q1,p1)';
c1=FFGE(b1);
d1=sum(c1)/p1;
e1=mean(d1);
 l1=[l1 e1];%%%%均值
  end

 plot(l1)
 xlabel('完整交织起始点');
ylabel('均值');
% title('线性分组码同步估计')
