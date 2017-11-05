# Drawing-by-one-line

Drawing by one line

	从某一点出发,经过每条边一次且仅一次.
  
【参考程序】

const max=6;{顶点数为6}

type shuzu=array[1..max,1..max]of 0..max;

const a:shuzu		       {图的描述与定义 1:连通;0:不通}

       =((0,1,0,1,1,1),
       
	 (1,0,1,0,1,0),
   
	 (0,1,0,1,1,1),
   
	 (1,0,1,0,1,1),
   
	 (1,1,1,1,0,0),
   
	 (1,0,1,1,0,0));
   
var

   bianshu:array[1..max]of 0..max; {与每一条边相连的边数}
   
   path:array[0..1000]of integer;  {记录画法,只记录顶点}
   
   zongbianshu,ii,first,i,total:integer;
   
procedure output(dep:integer);	{输出各个顶点的画法顺序}


var sum,i,j:integer;

begin

     inc(total);
     
     writeln('total:',total);
     
     for i:=0 to dep do write(Path[i]);writeln;
     
end;

function ok(now,i:integer;var next:integer):boolean;{判断第I条连接边是否已行过}

var j,jj:integer;

begin

     j:=0; jj:=0;
     
     while jj<>i do begin  inc(j);if a[now,j]<>0 then inc(jj);end;
     
     next:=j;
     
      {判断当前顶点的第I条连接边的另一端是哪个顶点,找出后赋给NEXT传回}
      
     ok:=true;
     
     if (a[now,j]<>1)  then  ok:=false;  {A[I,J]=0:原本不通}
     
end;					 {	=2:曾走过}

procedure init; {初始化}

var i,j :integer;

begin

     total:=0;	{方案总数}
     
     zongbianshu:=0;	  {总边数}
     
     for i:=1 to max do
     
	 for j:=1 to max do
   
	     if a[i,j]<>0 then begin inc(bianshu[i]);inc(zongbianshu);end;
       
	     {求与每一边连接的边数bianshu[i]}
       
     zongbianshu:=zongbianshu div 2;  {图中的总边数}
     
end;


procedure find(dep,nowpoint:integer); {dep:画第几条边;nowpoint:现在所处的顶点}

var i,next,j:integer;

begin

     for i:=1 to bianshu[nowpoint] do	{与当前顶点有多少条相接,则有多少种走法}
     
	 if ok(nowpoint,i,next) then begin  {与当前顶点相接的第I条边可行吗?}
   
					     {如果可行,其求出另一端点是NEXT}
               
	    a[nowpoint,next]:=2; a[next,nowpoint]:=2; {置成已走过标志}
      
      
	    path[dep]:=next;			      {记录顶点,方便输出}
      
	    if dep < zongbianshu then find(dep+1,next)  {未搜索完每一条边}
      
			       else output(dep);
             
	    path[dep]:=0;			      {回溯}
      
	    a[nowpoint,next]:=1; a[next,nowpoint]:=1;
      
	 end;
   
   
begin

   init;   {初始化,求边数等}
   
   for first:=1 to max do {分别从各个顶点出发,尝试一笔画}
   
     fillchar(path,sizeof(path),0);
     
     path[0]:=first;		{记录其起始的顶点}
     
     writeln('from point ',first,':');readln;
     
     find(1,first); {从起始点first,一条边一条边地画下去}
     
end.
