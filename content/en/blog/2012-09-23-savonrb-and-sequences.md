---
layout: post
title: "Savon.rb and sequences"
date: 2012-09-23 11:45:46 UTC
updated: 2012-09-23 11:45:46 UTC
comments: false
categories: [Developer, got ya]
---

As I wrote in my last post, I've recently learned <a href="http://gonfva.blogspot.com/2012/08/a-great-experience-applying-to-position_14.html" target="_blank">it is important to be able to show what you've been doing</a>. So&nbsp;I'm trying to increase the posts on a number of small problems I find in my daily job, and in my hobby projects. This one appeared in my daily job.
<br /><br />
I was developing a small interface between two applications via a SOAP web service. Actually, we are using <a href="http://www.redmine.org/" target="_blank">Redmine</a> for development, and <a href="http://www.bmc.com/products/product-listing/22735072-106757-2391.html" target="_blank">Remedy</a> for the whole organization, and we didn't want to fill the information twice.
<br /><br />
I was using <a href="http://savonrb.com/">Savon.rb</a> for creating a client in Redmine that would invoke a SOAP web service in Remedy. The code was something like this:
<br /><br />
<code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;response=client.request "Resolver" do<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;soap.header={:AuthenticationInfo=&gt;{<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; :userName=&gt;ws_user,<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; :password=&gt;ws_pwd,<br />&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; }<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;soap.body = {<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Status"=&gt;"Resolved",<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Resolution"=&gt; message&nbsp;,<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Status_Reason"=&gt;"Client Action Required",<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Incident_Number"=&gt;code,<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Status_Reason2"=&gt;"Client Action Required",<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"AssigneeLoginID"=&gt;user,<br />&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; }<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;end </code>
<br /><br />
However, I was getting a SOAP error
<br /><br />
<blockquote class="tr_bq">ARERR [8962] Unexpected element encountered in the input XML document</blockquote><br />The problem lies in <a href="https://github.com/rubiii/savon/issues/129">Ruby hashes not preserving the order and Savon sending XML sequences in incorrect order</a>. I just updated the call to something like this
<br /><br />
<code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;response=client.request "Resolver" do<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;soap.header={:AuthenticationInfo=&gt;{<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; :userName=&gt;ws_user,<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; :password=&gt;ws_pwd,<br /><b><span style="color: red;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; :order! =&gt; [:userName, :password]}</span></b>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;soap.body = {<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Status"=&gt;"Resolved",<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Resolution"=&gt;message,<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Status_Reason"=&gt;"Client Action Required",<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Incident_Number"=&gt;code,<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Status_Reason2"=&gt;"Client Action Required",<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"AssigneeLoginID"=&gt;user,<br /><b><span style="color: red;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;:order! =&gt; ["Status", "Resolution", "Status_Reason","Incident_Number", "Status_Reason2", "AssigneeLoginID"]</span></b>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;end </code>
<br /><br />
and it solved the issue. I expected Savon to take care of this, but anyway,&nbsp;I understand this is solved in Ruby 1.9x.
<br /><br />