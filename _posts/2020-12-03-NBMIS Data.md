---
layout: post
title: NBMIS Data
date: 2020-12-02 13:32:20 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: post-1.jpg # Add image post (optional)
tags: [Blog, Data]
author: Starry # Add name author (optional)

---

* TOC
{:toc}


# 更新数据（2021-01-08）

## 按保单号查询

{% highlight ruby %}

{

"sysNo" :"1",
		"provinceBranchNo" :"120000",
		"dataSource":"",
		"qry_opt":"0",
		"reqSysCode":"NBMIS",
		"pageNum":"1",
		"cntrNo":"2002120109S71000078904"
		}

{
			"sysNo" :"1",
			"provinceBranchNo" :"120000",
			"dataSource":"",
			qry_opt":"0",
			"reqSysCode":"NBMIS",
			"pageNum":"1",
			"cntrNo":"2002120109S71000080000"
		}

{% endhighlight %}

## 按网点号查询

{% highlight ruby %}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"qry_opt":"1",
"reqSysCode":"NBMIS",
"pageNum":"1",
"branchNo":"120221",
"agencyNo":"12022111020501"
}
{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"qry_opt":"1",
"reqSysCode":"NBMIS",
"pageNum":"1",
"branchNo":"120221",
"agencyNo":"12022181000047"
}
{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"qry_opt":"1",
"reqSysCode":"NBMIS",
"pageNum":"1",
"branchNo":"120221",
"agencyNo":"12022181000016"
}

{% endhighlight %}

## 按满期日期查询

{% highlight ruby %}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"qry_opt":"2",
"reqSysCode":"NBMIS",
"pageNum":"1",
"branchNo":"120000",
"insexprtStartDate":"2020-10-01",
"insexprtEndDate":"2021-03-01"
}
{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"qry_opt":"2",
"reqSysCode":"NBMIS",
"pageNum":"1",
"branchNo":"120221",
"insexprtStartDate":"2021-01-08",
"insexprtEndDate":"2021-08-01"
}

{% endhighlight %}



## 按营销员查询

{% highlight ruby %}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"ext04":"APP",
"qry_opt":"3",
"reqSysCode":"NBMIS",
"pageNum":"1",
"branchNo":"120221",
"agencyNo":"12022181000047"
}
{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"ext04":"APP",
"qry_opt":"3",
"reqSysCode":"NBMIS",
"pageNum":"1",
"branchNo":"120221",
"agencyNo":"12022181000016"
}
{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"ext04":"APP",
"qry_opt":"3",
"reqSysCode":"NBMIS",
"pageNum":"1",
"branchNo":"120109",
"agencyNo":"12010981000032"
}

{% endhighlight %}

**Update数据没变，还能使用**

# NBMIS保单查询

**Note：**

- **如果不对的话，将"agencyNo"改成 "branchNo"（六位）+"agencyNo"（八位，不够前面补0）格式**

- 必须要有的字段为：

  {% highlight ruby %}

  {

  "sysNo" :"1",

  "provinceBranchNo" :"120000",

  "dataSource":"",

  "qry_opt":"?",

  "reqSysCode":"NBMIS",

  "pageNum":"1"

  }

  {% endhighlight %}

  其他字段请根据查询酌情增加

## 按保单号查询

```
   SELECT
          a.cntr_no,
          a.pol_code,
          b.ipsn_no
   FROM std_contract a,cntr_basic_state b
   WHERE a.cntr_id=b.cntr_id
   and (master_cntr_id in (select cntr_id from std_contract where  sales_channel in ( 'SP' ,'OA') and cntr_stat not in ('O','T')))
   UNION
   SELECT
          a.cntr_no,
          a.pol_code,
          b.ipsn_no
   FROM std_contract a,cntr_basic_state b
   WHERE a.cntr_id=b.cntr_id
   and sales_channel in ( 'SP', 'OA') and a.cntr_stat not in ('O','T');
```
### 八版青岛：

{% highlight ruby %}
{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"1999370212S40000000406",
"qry_opt":"0",
"pageNum":"1"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"1999370212S40000002116",
"qry_opt":"0",
"pageNum":"1"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"1999370212S40000005443",
"qry_opt":"0",
"pageNum":"1"
}

{% endhighlight %}



### 八版天津：

{% highlight ruby %}
{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2001120000561015000022",
"qry_opt":"0",
"pageNum":"1"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2002120109S71000077798",
"qry_opt":"0",
"pageNum":"1"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2001120000561015000022",
"qry_opt":"0",
"pageNum":"1"
}

{% endhighlight %}




## 按营销员查询

```
SELECT  distinct
                          c.cntr_no,a.mclerk_no,a.mclerk_branch_no,
                          d.ipsn_no
                     FROM agent_post_mclerk a,agent_post_reg b,
                          std_contract c,     cntr_basic_state d
                     WHERE  a.agnet_post_id=b.agnet_post_id
                     and b.agnet_post_branch=c.n_sales_branch_no
                     and b.agent_post_no=c.n_sales_code
                     and c.cntr_id = d.cntr_id
                     and c.sales_channel in ('SP', 'OA') and c.cntr_stat not in ('O','T')
```

### 八版青岛：

{% highlight ruby %}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2003370210S64000001772",
"qry_opt":"1",
"branchNo":"370210",
"agencyNo":"82001063",
"pageNum":"1"

}
{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2003370210S64000004825",
"qry_opt":"1",
"branchNo":"370221",
"agencyNo":"U13",
"pageNum":"1"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2003370210S64000052844",
"qry_opt":"1",
"branchNo":"370210",
"agencyNo":"81001101",
"pageNum":"1"
}

{% endhighlight %}

### 八版天津：

{% highlight ruby %}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2006120111S81015000035 ",
"qry_opt":"1",
"branchNo":"120111 ",
"agencyNo":"81000042",
"pageNum":"1"

}
{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2006120107S81015000025 ",
"qry_opt":"1",
"branchNo":"120901",
"agencyNo":"81001010",
"pageNum":"1"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2006120231S81015000151 ",
"qry_opt":"1",
"branchNo":"120232 ",
"agencyNo":"zj0074",
"pageNum":"1"
}

{% endhighlight %}

## 按满期日期查询

```
SELECT a.cntr_no,a.cntr_expiry_date,a.n_sales_branch_no,
                                       b.ipsn_no
                                 FROM std_contract a,cntr_basic_state b
                                WHERE a.cntr_id=b.cntr_id
                                  and sales_channel in ('SP', 'OA') and a.cntr_stat not in ('O','T');
```

保证cntr_expiry_date在 start_date和end_date之间即可

**Note** 天津的测试数据库仅仅修复了 2020-10-01至2021-04-10之间的数据，共229条

### 八版青岛：

{% highlight ruby %}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2004370210S74000031180",
"qry_opt":"2",
"branchNo":"370210",
"CNTR_EXPIRY_DATE":"2009-08-10 00:00:00"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2003370220S64000003100",
"qry_opt":"2",
"branchNo":"370220",
"CNTR_EXPIRY_DATE":"2008-01-12 00:00:00"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2003370221S64000007961",
"qry_opt":"2",
"branchNo":"370221",
"CNTR_EXPIRY_DATE":"2008-04-24 00:00:00"
}

{% endhighlight %}

### 八版天津：

{% highlight ruby %}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"qry_opt":"2",
"branchNo":"120224",
"reqSysCode":"NBMIS",
"pageNum":"1",
"insexprtStartDate":"2020-10-01",
"insexprtEndDate":"2021-04-10"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"qry_opt":"2",
"branchNo":"120111",
"reqSysCode":"NBMIS",
"pageNum":"1",
"insexprtStartDate":"2020-10-01",
"insexprtEndDate":"2021-04-10"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"qry_opt":"2",
"branchNo":"120221",
"reqSysCode":"NBMIS",
"pageNum":"1",
"insexprtStartDate":"2020-10-01",
"insexprtEndDate":"2021-04-10"
}

{% endhighlight %}

## 按网点号查询

```
SELECT distinct c.cntr_no,b.agnet_post_branch,b.agent_post_no FROM agent_post_reg b,
                          std_contract c,     cntr_basic_state d
                     WHERE 
                     b.agnet_post_branch=c.n_sales_branch_no
                     and b.agent_post_no=c.n_sales_code
                     and b.channel_type in ('B','#')
                     and c.cntr_id = d.cntr_id
                     and c.sales_channel in ('SP', 'OA') c.cntr_stat not in ('O','T');
```
### 八版青岛：

{% highlight ruby %}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2003370210S64000000906",
"qry_opt":"3",
"branchNo":"370210",
"agencyNo":"11020807",
"pageNum":"1"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2003370210S64000052844",
"qry_opt":"3",
"branchNo":"370212",
"agencyNo":"12001011",
"pageNum":"1"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"1999370212S43000003237",
"qry_opt":"3",
"branchNo":"370212",
"agencyNo":"82001233",
"pageNum":"1"
}

{% endhighlight %}



### 八版天津：

{% highlight ruby %}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2006120231S74015000043 ",
"qry_opt":"3",
"branchNo":"120232 ",
"agencyNo":"11010154",
"pageNum":"1"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2006120231S81015000067 ",
"qry_opt":"3",
"branchNo":"120232",
"agencyNo":"11018442",
"pageNum":"1"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2006120221S81015000019 ",
"qry_opt":"3",
"branchNo":"120221",
"agencyNo":"11020507",
"pageNum":"1"
}

{% endhighlight %}

## 注意事项

测试环境有些数据不对：

1. 可能`O_Sales_code`或者`N_Sales_code`不是八位。

2. 在`AGENT_POST_MCLERK`表下面有些缺乏自营网点。目前会在`Error_Message`里面提示对应的保单号（老接口没有此逻辑)，需要修改测试数据，例如：
   - Step 1
   
      ```sql
      select * from cl_biz1.agent_post_reg where agent_post_branch = '120231'
      ```
     找到一个自营网点号，例如`10810` 
   - step 2
   
     ```sql
     select * from cl_biz1.AGENT_POST_MCLERK where mclerk_branch_no = '120231' and mclerk_no = '092'
     ```
     
   - Step 3
   
     ```sql
     insert into cl_biz1.GENT_POST_MCLERK values('10810','120231','092','1200008230')
     ```
   
     

# NBMIS更新营销员

{% highlight ruby %}
{
"provBranchNo" :"120000",
"reqSysCode" :"NBMIS",
"eiqAgencyCntrInfoList":[{
"sysNo" :"1",
"cntrNo":"2011120111S660150173199",
"polCode":"22",
"agencyBranchNo":"110103",
"agencyNo":"11010300000B05"
},
{
"sysNo" :"1",
"cntrNo":"2011120111454015017391",
"polCode":"454",
"agencyBranchNo":"120000",
"agencyNo":"12000000001012"
},
{
"sysNo" :"1",
"cntrNo":"2011120232453015009632",
"polCode":"453",
"agencyBranchNo":"120000",
"agencyNo":"12000011200537"
},
{
"sysNo" :"6",
"cntrNo":"1997410300SA9000087248",
"polCode":"SA9",
"agencyBranchNo":"120000",
"agencyNo":"120000111L5264"
}]
}
	
{% endhighlight %}

**NOTE**
- 目前测试了 "provBranchNo" :"120000"（sysNo" :"1" 的是天津的数据库）
- 后俩数据是有主险和附加险的数据