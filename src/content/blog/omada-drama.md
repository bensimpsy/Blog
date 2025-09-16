---
title: 'Omada Drama'
description: 'Speed issues with a TP-Link ER605 Omada Router'
pubDate: 'Sep 17, 2025'
---

I've read on several occasions people blogging technical issues they have encountered, and some time later googling for the same issue and coming across their own blog post.  I'm writing this hoping it helps someone else, I really hope I don't end up on my own blog with this same issue down the track.

Recently the National Broadband Network (NBN) in Australia gave everyone on a Fibre to the Premises (FTTP) plan of 100Mbps or more a nice healthy speed bump.  I was on a 100Mbps Down, 20Mpbs Up plan with Aussie Broadband (highly recommend) and got a nice bump to 500Mbps Down, 50 Mbps Up. Glorious.  I don't even think I took advantage of the 100/20 speeds, but I'm an 'all the gear, no idea' type of guy so the thought of having those speeds available made me happy.

So, the new speeds apply and the first thing I (like any nerd) do is run a speed test and to my surprise I'm capping out at around 255Mbps Down, 53 Up.  Bit shocked, Aussie Broadband have only been good to me, and to their credit they are quite open with there <a href="https://www.aussiebroadband.com.au/network/cvc-graphs/" target="_blank">CVC Graphs</a> (CVC stands for Connectivity Virtual Circuit and is explained in the preceding link).  I checked the graph out and can see they have plenty of headroom, so its unlikely them. The next likely culprit was either my hardware, of the NBN infrastructure.  

I run TP-Link hardware managed by the Omada platform.  I have a <a href="https://www.tp-link.com/au/business-networking/omada-sdn-switch/er605/" target="_blank">TP-Link ER605 Router</a>, connected to an unmanaged TP-Link PoE switch, with 2 wireless access points (one in my house, and the other in an outside office).  The setup is overkill for home, but it does allow some good flexibility compared to all-in-one modem/routers that I used use and most sane households would do.  The main reasons is that it allows my 2 access points to run as a mesh network allowing seamless handoff.  Another is that when new wireless technologies come out I only have to update the access points which is cheaper than replacing an all-in-one.  The final reason is I can stash my router, switch and patch panel away in a closet, have my access point mounted in a central spot in the house giving me good wireless coverage, compared to the all-in-one stashed in the closet affecting range.

I then do what it seems like most people do now, visit ChatGPT.  I outline my network topology and equipment and explain my issue, and to my surprise it comes back with an answer explaining what could be causing my issue (spoiler: it was correct).  
It suggests that my router might have reverted to using the CPU to manage NAT, rather than it's dedicated hardware, and the CPU is throttling the WAN connection to 250Mbps. It seems to occur when certain features such as QoS (Quality of Service), Client Statistics and others are enabled, and the combination disables hardware NAT.  I'm very aware of LLM hallucinations, so I'm skeptical of this answer.  I ask it for sources to this theory, and to its credit it references several blog posts regarding NAT functionality, but ultimately identifies that none of the references specifically mention this issue. Despite this, it is adamant that this is the cause.

It did provide a number of workarounds and troubleshooting:
1. Buy a better router (lol)
2. Connect a PC directly to the NTU (Network Termination Device) and run a speedtest (validating that the router or other client equipment is the problem)
3. Remove the router from Omada management, run it in standalone mode, disabling the services pushing the router to using CPU NAT and reverting to hardware NAT.

I did what any speed-chasing technical person would do, start messing with my network while my wife was watching Netflix.  I removed the router from Omada management and reset it to factory defaults. Reconfigured it to run in standalone mode and re-ran my speed test and voila - 537Mbps down. Downtime of around 5 minutes.

So there we have it.  If you have a TP-Link ER605 Router, and have an internet speed capped at 250Mbps, this is probably why.  Its frustrating this isn't documented anywhere, I've essentially lost most of the nice Omada management features now and I probably wouldn't have purchased this unit had I known of this limitation.

Just a side note on ISP's (Internet Service Providers), good ones are few and far between and Aussie Broadband is one of the good ones.  Think Internode of the 2000's, or Adam Internet if your an Adelaide local.  Coming from someone who's had to deal with internet issues for countless family and friends - good local support is worth spending the extra money on, and they have it. If you think of the Project Management Triangle 'Good, Fast, Cheap - Pick 2', and you've picked Good and Fast, then you will love Aussie Broadband. You can use my referral code <strong>1710276</strong> and we both get $50 off your first month. Win-Win!