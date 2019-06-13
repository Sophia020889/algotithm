# 1.回文数

```
?php
/**
 * Created by PhpStorm.
 * Date: 2019-06-12
 * Time: 09:35
 * desc:判断一个整数是否是回文数。回文数是指(从左向右)和倒序(从右向左)读都是一样的整数
 * thought：1.负数不是回文数，正数的倒序数值与此正数相等则是回文数
 */

//方法一：将输入的数字转化为字符串，利用循环进行倒序输出，再根据php对类型不敏感性，比较原数与逆序数是否相等
function IsPalindrome($num)
{
    if($num < 0)//负数不符合回文数要求
        return false;
    $num = strval($num);
    $len = strlen($num);
    $new_num = '';
    for($i = $len - 1; $i >= 0 ;$i--)
    {
        $new_num .= $num[$i];
    }
    if($num == $new_num)
        return true;
    return false;
}
/*var_dump(IsPalindrome(-12));
var_dump(IsPalindrome(10));
var_dump(IsPalindrome(121));*/

//方法二：根据数组个十百的特性，整除+取余求出数的倒序值
function InvertedNum($num)
{
    $cur = 0;
    while($num != 0 )
    {
        $cur = $cur * 10 + $num % 10;
        $num  = intval($num / 10);
    }
    return $cur;
}
function IsPalindrome2($num)
{
    if($num < 0)//负数不符合回文数要求
        return false;
    $new_num =InvertedNum($num);
    //var_dump($new_num);
    if($num == $new_num)
        return true;
    return false;
}
var_dump(IsPalindrome2(-12));
var_dump(IsPalindrome2(10));
var_dump(IsPalindrome2(121));
var_dump(IsPalindrome2(123));
?>
```

# 2.给定一个数组nums, 写一个函数，将数组中所有的0挪到数组的末尾，而维持其他所有非0元素的相对值

```
<?php
/**
 *  给定一个数组nums, 写一个函数，将数组中所有的0挪到数组的末尾，而维持其他所有非0元素的相对值。
 *
 *  举例： nums= [0,1,0,2,3]  函数运行后结果为：[1,2,3,0,0]
 *
 */

$Data = array(0,1,0,2,3);
function MoveZeros($input)
{
    $ori_len = count($input);
    $resArray = array();
    foreach ($input as $value)
    {
        if($value)
            $resArray[] = $value;
    }
    $new_len = count($resArray);
    $numOfZero =  $ori_len - $new_len;
    for($i = 0;$i < $numOfZero;$i++)
        $resArray[] = 0;
    return $resArray;

}
var_dump(MoveZeros($Data));

?>
```

#3.给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。

```
<?php
/*给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。

    /*
    思路：题目要求B的i个元素等于A中除了i个元素所以元素乘积
    因此思路很清晰，双重循环：
    第一层循环表示将要相乘A的元素；
    第二层循环表示B中的元素要乘上A的元素（判断条件：i==j则直接跳过）
*/
function multiply($AArray=array())
{
    $len = count($AArray);
    $BArray = array();
    if($len <= 1)
        return $BArray;
    for($i = 0; $i < $len; $i ++)
    {
        $BArray[$i] = 1;
        for($j = 0; $j < $len; $j++)
        {
            if($i == $j)
                continue;
            //$tmp *= $AArray[$j];
            $BArray[$i] *= $AArray[$j];
        }
    }
    return $BArray;
}
//$A_array = array(3,2,5,6,3);
//$A_array = array();
$A_array = array(3);
//$A_array = array(3,2);
$B_array = multiply($A_array);
var_dump($B_array);
?>
```

# 4.把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素

```
<?php
/*
 把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素
例如，数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1
*/
//$data = array(3,4,5,1,2);
//$data = array(1,2,3,4,5);
//$data = array(1,0,1,1,1);
//$data = array(1,1,1,0,1);
$data = array(4,5,6,2,2,2,2,2,3,3,3,3,3,3);

function GetRevoleMin($input,$length)
{
    if(empty($input) || $length <= 0)
        return 0;
    $index1 = $indexMid = 0;//如果只旋转0个数组，那么第一个玄素就是最小值
    $index2 = $length - 1;
    while($input[$index1] >=  $input[$index2])//旋转元素个数>=1
    {
        if(($index2 - $index1) == 1 )
        {
            $indexMid = $index2;
            break;
        }
        if($input[$index1] == $input[$index2] && $input[$indexMid] == $input[$index1])
            return MinINOrder($input,$index1,$index2);
        $indexMid = intval(($index1 + $index2)/2);//取整，否则死循环
        if($input[$indexMid] >= $input[$index1])
            $index1 = $indexMid;
        elseif($input[$indexMid] <= $input[$index2])
            $index2 = $indexMid;
    }
    return $input[$indexMid];
}
$MIN = GetRevoleMin($data,count($data));
echo $MIN;
function MinINOrder($nums,$index1,$index2)//重复数字时必须用顺序查找
{
    $result = $nums[$index1];
    for($i = $index1 + 1;$i < $index2; ++$i)
    {
        if($result > $nums[$i])
            $result = $nums[$i];//返回更小的
    }
    return $result;
}
?>
```

# 5.在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。

```
<?php
/*
* 题目：在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。
* 请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
*/

$data = array(
    array(1,4,7,11,15),
    array(2,5,8,12,19),
    array(3,6,9,16,22),
    array(10,13,14,17,24),
    array(18,21,23,26,30),
);
function SearchNumOfTwoArray($search,$input)
{
    if(empty($input))
        return false;
    $rows = count($input);
    $cols = count($input[0]);
    $index_row = 0;
    $index_col = $cols - 1;
    while($index_row <= $rows - 1 && $index_col >= 0)
    {
        $num = $input[$index_row][$index_col];
        var_dump($num);
        if($search < $num)
            $index_col--;
        elseif($search > $num)
            $index_row++;
        else
            return true;

    }
    return false;
}

$res = SearchNumOfTwoArray(45,$data);
var_dump($res);
?>
```

# 6.求连续子数组的最大和

```
<?php
$Data = array(1,-2,3,10,-4,7,2,-5);
$Data1 = array(-10,-2,-3,-4,-7,-1,-5);

var_dump(SubArrayMaxSum($Data));
var_dump(SubArrayMaxSum($Data1));

function SubArrayMaxSum($DataInput)
{
   if(empty($DataInput))
       return 0;
   $len = count($DataInput);
   $greatsum = max($DataInput);//兼容全是负数逻辑
   $curSum= $DataInput[0];
   for($i = 1; $i < $len;$i++)
   {
       $curSum = $curSum + $DataInput[$i];
       /*if($DataInput[$i] > $greatsum)//处理负数
       {
           $greatsum = $DataInput[$i];
       }*/
       if($curSum < 0)//当前和小于-，再加正数或者负数都会小于当前数，清空$curSum
       {
           $curSum = 0;
           continue;
       }
       if($curSum > $greatsum)
       {
           $greatsum = $curSum;
       }
   }
   return $greatsum;
}
?>
```