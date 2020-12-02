---
layout: post
title: 引擎搜索技巧
date: 2020-12-02 13:32:20 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: post-5.jpg # Add image post (optional)
tags: [Blog, Data]
author: Starry # Add name author (optional)

---

* TOC
{:toc}
**Note： 如果不对的话，将"agencyNo"改成 "branchNo"（六位）+"agencyNo"（八位，不够前面补0）格式 **


# 按保单号查询

```
                     
SELECT
          a.cntr_no,
          a.pol_code,
          b.ipsn_no
   FROM std_contract a,cntr_basic_state b
   WHERE a.cntr_id=b.cntr_id
   and (master_cntr_id in (select cntr_id from std_contract where  sales_channel in ( 'SP' ,'OA')))
   UNION
   SELECT
          a.cntr_no,
          a.pol_code,
          b.ipsn_no
   FROM std_contract a,cntr_basic_state b
   WHERE a.cntr_id=b.cntr_id
   and sales_channel in ( 'SP', 'OA');
```
{% highlight ruby %}
{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2000120000R34300000023",
"qry_opt":"0",
"pageNum":"1"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2000120000SI4015000027",
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


# 按营销员查询

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
                     and c.sales_channel in ('SP', 'OA')
```

{% highlight ruby %}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2006120107S81015000025",
"qry_opt":"1",
"branchNo":"120901",
"agencyNo":"81001010",
"pageNum":"1"

}
{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2006120113S81015000150",
"qry_opt":"1",
"branchNo":"120113",
"agencyNo":"81000006",
"pageNum":"1"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2006120225S81015000026",
"qry_opt":"1",
"branchNo":"120225",
"agencyNo":"82000096",
"pageNum":"1"
}

{% endhighlight %}

# 按满期日期查询

```
SELECT a.cntr_no,a.cntr_expiry_date,a.n_sales_branch_no,
                                       b.ipsn_no
                                 FROM std_contract a,cntr_basic_state b
                                WHERE a.cntr_id=b.cntr_id
                                  and sales_channel in ('SP', 'OA');
```

保证cntr_expiry_date在 start_date和end_date之间即可

{% highlight ruby %}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2006120107S81015000025",
"qry_opt":"2",
"branchNo":"120901",
"CNTR_EXPIRY_DATE":"2011-01-04 00:00:00"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2006120107S81015000038",
"qry_opt":"2",
"branchNo":"120901",
"CNTR_EXPIRY_DATE":"2011-01-04 00:00:00"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2006120224S81015000313",
"qry_opt":"2",
"branchNo":"120224",
"CNTR_EXPIRY_DATE":"2011-01-04 00:00:00"
}

{% endhighlight %}

# 按网点号查询

```
SELECT distinct c.cntr_no,b.agnet_post_branch,b.agent_post_no FROM agent_post_reg b,
                          std_contract c,     cntr_basic_state d
                     WHERE 
                     b.agnet_post_branch=c.n_sales_branch_no
                     and b.agent_post_no=c.n_sales_code
                     and b.channel_type in ('B','#')
                     and c.cntr_id = d.cntr_id
                     and c.sales_channel in ('SP', 'OA');
```
{% highlight ruby %}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2006120112S81015000010",
"qry_opt":"3",
"branchNo":"120112",
"agencyNo":"11018281",
"pageNum":"1"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2006120107S81015000041",
"qry_opt":"3",
"branchNo":"120901",
"agencyNo":"12007004",
"pageNum":"1"
}

{
"sysNo" :"1",
"provinceBranchNo" :"120000",
"dataSource":"",
"cntrNo":"2006120225S81015000107",
"qry_opt":"3",
"branchNo":"120225",
"agencyNo":"12008001",
"pageNum":"1"
}

{% endhighlight %}