---
layout: post
previous: https://gonfva.blogspot.com/2012/08/savonrb-and-sequences.html
title: "Savon.rb and sequences"
date: 2012-08-15T11:45:46
comments: false
categories: [Developer, got ya]
tags:
  - Developer
---

As I wrote in my last post, I've recently learned [it is important to be able to show what you've been doing](https://gonfva.blogspot.com/2012/08/a-great-experience-applying-to-position_14.html). So I'm trying to increase the posts on a number of small problems I find in my daily job, and in my hobby projects. This one appeared in my daily job.


I was developing a small interface between two applications via a SOAP web service. Actually, we are using [Redmine](http://www.redmine.org/) for development, and [Remedy](http://www.bmc.com/products/product-listing/22735072-106757-2391.html) for the whole organization, and we didn't want to fill the information twice.


I was using [Savon.rb](http://savonrb.com/) for creating a client in Redmine that would invoke a SOAP web service in Remedy. The code was something like this:


<code>          response=client.request "Resolver" do
            soap.header={:AuthenticationInfo=>{
                 :userName=>ws_user,
                 :password=>ws_pwd,
            }
            soap.body = {
                "Status"=>"Resolved",
                "Resolution"=> message ,
                "Status_Reason"=>"Client Action Required",
                "Incident_Number"=>code,
                "Status_Reason2"=>"Client Action Required",
                "AssigneeLoginID"=>user,
            }
          end </code>


However, I was getting a SOAP error



<blockquote class="tr_bq">ARERR [8962] Unexpected element encountered in the input XML document</blockquote>
The problem lies in [Ruby hashes not preserving the order and Savon sending XML sequences in incorrect order](https://github.com/rubiii/savon/issues/129). I just updated the call to something like this


<code>          response=client.request "Resolver" do
            soap.header={:AuthenticationInfo=>{
                 :userName=>ws_user,
                 :password=>ws_pwd,
 **<span style="color: red;">                 :order! => [:userName, :password]}</span>**            }
            soap.body = {
                "Status"=>"Resolved",
                "Resolution"=>message,
                "Status_Reason"=>"Client Action Required",
                "Incident_Number"=>code,
                "Status_Reason2"=>"Client Action Required",
                "AssigneeLoginID"=>user,
 **<span style="color: red;">                :order! => ["Status", "Resolution", "Status_Reason","Incident_Number", "Status_Reason2", "AssigneeLoginID"]</span>**            }
          end </code>


and it solved the issue. I expected Savon to take care of this, but anyway, I understand this is solved in Ruby 1.9x.
