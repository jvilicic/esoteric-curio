---
title: "MySQL: the best database I'm afraid to use"
date: 2004-02-05 02:45:00
type: post
categories:
- Damaged Bits
tags:
- mysql
---

So, there are a lot of databases out there.  I've used more than a handful.  Commercial, Proprietary, Free, Open, etc.  The two databases I've used most are Oracle and MySQL and let me say that I like them both immensely.  <p>However, recently we considered putting database support into a commercial, proprietary application and MySQL concerned and confused me.</p>  <p>Recently, <a href="http://spf.pobox.com/">SPFv1</a> has been popular -- likely due to the recent publication of SPF records by Internet giant AOL.  SPF stands for Sender Permitted From and allows ISPs and other domain owners to publish a detailed "allowable senders" list through DNS.</p>  <p>This helps stop forged spam.  As a great technology and simple, flexible specificaion it was ideal to include into <a href="http://www.omniti.com/solutions/ecelerity.php">Ecelerity</a>.  Ecelerity can handle 100,000+ concurrent inbound connections and trigger time-costs (teergrube/tarpitting) based on RBLs results, SPFv1 results and pretty much anything else you can think of and implement in Java, C or Perl.  Pretty cool! :-D</p>  <p>On the SPF mailing list, someone discussed a mechanism for SPF enabled mail exchanges to consolidate their failure logs in a central location using features in MySQL.  I thought this was a very clever idea.  Which leads me to my one beef with MySQL</p>  <p>MySQL's stance on their license doesn't match the wording of the license.  Now, many people I've talked to have asked "why not purchase a commercial license?"  First let me say, we have a lot of commercial licenses to various products.  It is neither cost nor principle that deters me from purchasing a MySQL commercial license to resell with a product using it.  Confusion deters me.  Here is a copy of my posting to the SPF mailing list:</p>  <p style="font-style: italic"> MySQL's license doesn't says I can't write my own MySQL client library to log these things and distribute it with our product.  However, the people at MySQL maintain their license (GPL) prohibits this.  Something about the GPL applying to the use of the database and that any access of the database would violate the GPL.  Last time I checked, the GPL is a copyright and they can't copyright an idea nor my work -- which leaves them out of luck.</p>  <p style="font-style: italic"> I wish they would amend their license with something like: "you must distribute a commercial client license with any product released under an non-GPL compatible license that accesses a non-commercially licensed MySQL database."  Then I would just buy licenses.  As it is now, their stance concerns and confuses me and I tend not to purchase things while concerned or confused.</p>  <p style="font-style: italic"> Sigh.  I really like MySQL.  I use it internally for many things.  I just need MySQL to align their "free" license with their intentions so that everyone involved understands their rights. </p> 