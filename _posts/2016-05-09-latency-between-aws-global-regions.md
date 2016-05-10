---
title: Latency Between AWS Global Regions
---

I was wondering the accurate latency between all AWS global regions, so I did this test.

Please note this test does not include **China (Beijing) \| cn-north-1** because it is a complete isolated region with separate domains and account system.

I used [some bash script][1] to make launching/terminating instances across regions easier. 

## Setup

Instances in all regions were `c4.large` running the lastest [Ubuntu Server 16.04 LTS (Xenial)][2] except **South America (São Paulo)**:

- `c3.large` was used because `c4.large` was not supported yet
- [Ubuntu Server 14.04 LTS (Trusty)][3] was used because I got [Kernal panic][4] booting Ubuntu 16.04

All ping tests were done by:

    ping -c 60 [host]

## Results

### US East (N. Virginia) | us-east-1

|         Region Name       |      Region    |   min   |     avg     |   max   | stddev  |
|---------------------------|----------------|---------|-------------|---------|---------|
| US West (N. California)   | us-west-1      | 72.708  | **72.738**  | 72.799  | *0.241* |
| US West (Oregon)          | us-west-2      | 86.880  | **86.981**  | 87.527  | *0.328* |
| EU (Ireland)              | eu-west-1      | 80.512  | **80.546**  | 80.730  | *0.378* |
| EU (Frankfurt)            | eu-central-1   | 88.628  | **88.657**  | 88.696  | *0.388* |
| Asia Pacific (Tokyo)      | ap-northeast-1 | 145.212 | **145.255** | 145.536 | *0.281* |
| Asia Pacific (Seoul)      | ap-northeast-2 | 176.127 | **178.421** | 180.513 | *0.698* |
| Asia Pacific (Singapore)  | ap-southeast-1 | 216.641 | **216.719** | 217.830 | *0.316* |
| Asia Pacific (Sydney)     | ap-southeast-2 | 229.700 | **229.972** | 237.249 | *1.051* |
| South America (São Paulo) | sa-east-1      | 119.478 | **119.531** | 119.642 | *0.210* |

### US West (N. California) | us-west-1

|         Region Name       |      Region    |   min   |     avg     |   max   | stddev  |
|---------------------------|----------------|---------|-------------|---------|---------|
| US East (N. Virginia)     | us-east-1      | 71.590  | **71.632**  | 71.835  | *0.127* |
| US West (Oregon)          | us-west-2      | 19.435  | **19.464**  | 19.575  | *0.129* |
| EU (Ireland)              | eu-west-1      | 153.167 | **153.202** | 153.257 | *0.446* |
| EU (Frankfurt)            | eu-central-1   | 166.585 | **166.609** | 166.655 | *0.428* |
| Asia Pacific (Tokyo)      | ap-northeast-1 | 102.484 | **102.504** | 102.561 | *0.102* |
| Asia Pacific (Seoul)      | ap-northeast-2 | 131.494 | **131.513** | 131.589 | *0.468* |
| Asia Pacific (Singapore)  | ap-southeast-1 | 173.953 | **174.010** | 174.536 | *0.491* |
| Asia Pacific (Sydney)     | ap-southeast-2 | 157.421 | **157.463** | 157.879 | *0.552* |
| South America (São Paulo) | sa-east-1      | 192.639 | **192.670** | 192.768 | *0.446* |

### US West (Oregon) | us-west-2

|         Region Name       |      Region    |   min   |     avg     |   max   | stddev  |
|---------------------------|----------------|---------|-------------|---------|---------|
| US East (N. Virginia)     | us-east-1      | 88.414  | **88.683**  | 89.060  | *0.460* |
| US West (N. California)   | us-west-1      | 18.908  | **19.204**  | 19.517  | *0.265* |
| EU (Ireland)              | eu-west-1      | 136.941 | **136.979** | 137.046 | *0.118* |
| EU (Frankfurt)            | eu-central-1   | 159.494 | **159.523** | 159.566 | *0.242* |
| Asia Pacific (Tokyo)      | ap-northeast-1 | 89.050  | **89.095**  | 89.578  | *0.333* |
| Asia Pacific (Seoul)      | ap-northeast-2 | 118.286 | **118.312** | 118.374 | *0.421* |
| Asia Pacific (Singapore)  | ap-southeast-1 | 161.331 | **161.367** | 161.432 | *0.344* |
| Asia Pacific (Sydney)     | ap-southeast-2 | 162.152 | **162.175** | 162.303 | *0.540* |
| South America (São Paulo) | sa-east-1      | 182.691 | **182.716** | 182.793 | *0.374* |

### EU (Ireland) | eu-west-1

|         Region Name       |      Region    |   min   |     avg     |   max   | stddev  |
|---------------------------|----------------|---------|-------------|---------|---------|
| US East (N. Virginia)     | us-east-1      | 80.481  | **80.524**  | 80.683  | *0.321* |
| US West (N. California)   | us-west-1      | 153.192 | **153.220** | 153.286 | *0.226* |
| US West (Oregon)          | us-west-2      | 136.945 | **136.976** | 137.044 | *0.345* |
| EU (Frankfurt)            | eu-central-1   | 19.540  | **19.560**  | 19.616  | *0.188* |
| Asia Pacific (Tokyo)      | ap-northeast-1 | 212.343 | **212.388** | 212.519 | *0.484* |
| Asia Pacific (Seoul)      | ap-northeast-2 | 242.353 | **242.390** | 242.466 | *0.360* |
| Asia Pacific (Singapore)  | ap-southeast-1 | 210.647 | **239.023** | 247.894 | *7.279* |
| Asia Pacific (Sydney)     | ap-southeast-2 | 309.525 | **309.562** | 309.696 | *0.740* |
| South America (São Paulo) | sa-east-1      | 191.258 | **191.292** | 191.346 | *0.541* |

### EU (Frankfurt) | eu-central-1

|         Region Name       |      Region    |   min   |     avg     |   max   | stddev  |
|---------------------------|----------------|---------|-------------|---------|---------|
| US East (N. Virginia)     | us-east-1      | 88.559  | **88.624**  | 88.995  | *0.380* |
| US West (N. California)   | us-west-1      | 166.556 | **166.590** | 166.632 | *0.247* |
| US West (Oregon)          | us-west-2      | 159.493 | **159.542** | 159.902 | *0.269* |
| EU (Ireland)              | eu-west-1      | 19.513  | **19.553**  | 19.641  | *0.193* |
| Asia Pacific (Tokyo)      | ap-northeast-1 | 236.497 | **236.537** | 236.729 | *0.418* |
| Asia Pacific (Seoul)      | ap-northeast-2 | 264.835 | **266.154** | 268.761 | *1.965* |
| Asia Pacific (Singapore)  | ap-southeast-1 | 325.897 | **325.934** | 326.189 | *0.707* |
| Asia Pacific (Sydney)     | ap-southeast-2 | 323.181 | **323.483** | 327.678 | *1.125* |
| South America (São Paulo) | sa-east-1      | 194.882 | **194.905** | 194.937 | *0.509* |

### Asia Pacific (Tokyo) | ap-northeast-1

|         Region Name       |      Region    |   min   |     avg     |   max   | stddev  |
|---------------------------|----------------|---------|-------------|---------|---------|
| US East (N. Virginia)     | us-east-1      | 145.223 | **145.261** | 145.533 | *0.255* |
| US West (N. California)   | us-west-1      | 102.477 | **102.523** | 102.604 | *0.219* |
| US West (Oregon)          | us-west-2      | 89.061  | **89.157**  | 90.041  | *0.388* |
| EU (Ireland)              | eu-west-1      | 212.349 | **212.388** | 212.452 | *0.646* |
| EU (Frankfurt)            | eu-central-1   | 236.521 | **236.558** | 236.686 | *0.462* |
| Asia Pacific (Seoul)      | ap-northeast-2 | 31.393  | **31.438**  | 31.584  | *0.166* |
| Asia Pacific (Singapore)  | ap-southeast-1 | 73.762  | **73.785**  | 73.881  | *0.337* |
| Asia Pacific (Sydney)     | ap-southeast-2 | 103.873 | **103.907** | 104.091 | *0.369* |
| South America (São Paulo) | sa-east-1      | 256.664 | **256.763** | 257.904 | *0.512* |

### Asia Pacific (Seoul) | ap-northeast-2

|         Region Name       |      Region    |   min   |     avg     |   max   | stddev  |
|---------------------------|----------------|---------|-------------|---------|---------|
| US East (N. Virginia)     | us-east-1      | 176.569 | **176.606** | 176.866 | *0.222* |
| US West (N. California)   | us-west-1      | 132.580 | **132.615** | 132.658 | *0.345* |
| US West (Oregon)          | us-west-2      | 117.717 | **118.221** | 118.515 | *0.257* |
| EU (Ireland)              | eu-west-1      | 242.380 | **242.406** | 242.442 | *0.284* |
| EU (Frankfurt)            | eu-central-1   | 264.839 | **265.312** | 268.726 | *1.262* |
| Asia Pacific (Tokyo)      | ap-northeast-1 | 31.143  | **31.315**  | 31.889  | *0.275* |
| Asia Pacific (Singapore)  | ap-southeast-1 | 71.842  | **71.872**  | 71.998  | *0.184* |
| Asia Pacific (Sydney)     | ap-southeast-2 | 133.997 | **134.037** | 134.390 | *0.475* |
| South America (São Paulo) | sa-east-1      | 286.130 | **286.253** | 287.537 | *0.689* |

### Asia Pacific (Singapore) | ap-southeast-1

|         Region Name       |      Region    |   min   |     avg     |   max   | stddev  |
|---------------------------|----------------|---------|-------------|---------|---------|
| US East (N. Virginia)     | us-east-1      | 216.619 | **216.680** | 216.872 | *0.391* |
| US West (N. California)   | us-west-1      | 173.909 | **173.946** | 173.990 | *0.424* |
| US West (Oregon)          | us-west-2      | 161.380 | **161.423** | 161.731 | *0.510* |
| EU (Ireland)              | eu-west-1      | 218.757 | **238.130** | 248.009 | *7.282* |
| EU (Frankfurt)            | eu-central-1   | 325.881 | **325.918** | 325.991 | *0.634* |
| Asia Pacific (Tokyo)      | ap-northeast-1 | 73.774  | **73.807**  | 73.952  | *0.054* |
| Asia Pacific (Seoul)      | ap-northeast-2 | 67.390  | **71.291**  | 71.951  | *1.542* |
| Asia Pacific (Sydney)     | ap-southeast-2 | 175.304 | **175.328** | 175.470 | *0.334* |
| South America (São Paulo) | sa-east-1      | 328.042 | **328.080** | 328.114 | *0.572* |

### Asia Pacific (Sydney) | ap-southeast-2

|         Region Name       |      Region    |   min   |     avg     |   max   | stddev  |
|---------------------------|----------------|---------|-------------|---------|---------|
| US East (N. Virginia)     | us-east-1      | 229.673  | **229.748**  | 230.146  | *0.287* |
| US West (N. California)   | us-west-1      | 157.682  | **157.843**  | 159.061  | *0.466* |
| US West (Oregon)          | us-west-2      | 161.894  | **161.932**  | 161.985  | *0.447* |
| EU (Ireland)              | eu-west-1      | 309.266  | **309.562**  | 310.253  | *0.565* |
| EU (Frankfurt)            | eu-central-1   | 323.104  | **323.152**  | 323.733  | *0.774* |
| Asia Pacific (Tokyo)      | ap-northeast-1 | 103.860  | **103.900**  | 103.968  | *0.187* |
| Asia Pacific (Seoul)      | ap-northeast-2 | 134.006  | **134.025**  | 134.092  | *0.453* |
| Asia Pacific (Singapore)  | ap-southeast-1 | 175.281  | **175.355**  | 176.336  | *0.511* |
| South America (São Paulo) | sa-east-1      | 322.453  | **322.494**  | 322.604  | *0.789* |

### South America (São Paulo) | sa-east-1

|         Region Name       |      Region    |   min   |     avg     |   max   | stddev  |
|---------------------------|----------------|---------|-------------|---------|---------|
| US East (N. Virginia)     | us-east-1      | 119.506  | **119.542**  | 119.802  | *0.311* |
| US West (N. California)   | us-west-1      | 192.681  | **192.700**  | 192.782  | *0.015* |
| US West (Oregon)          | us-west-2      | 181.040  | **181.665**  | 182.719  | *0.897* |
| EU (Ireland)              | eu-west-1      | 191.264  | **191.559**  | 191.648  | *0.479* |
| EU (Frankfurt)            | eu-central-1   | 194.867  | **194.900**  | 194.987  | *0.228* |
| Asia Pacific (Tokyo)      | ap-northeast-1 | 256.541  | **256.665**  | 257.172  | *0.136* |
| Asia Pacific (Seoul)      | ap-northeast-2 | 286.142  | **286.272**  | 286.820  | *0.658* |
| Asia Pacific (Singapore)  | ap-southeast-1 | 327.886  | **327.924**  | 328.094  | *0.278* |
| Asia Pacific (Sydney)     | ap-southeast-2 | 322.494  | **322.523**  | 322.559  | *0.374* |

## Conclusions

As you can see, the latency between different AWS regions are quite steady. The jitter is below 1ms most of the time. I think it's because there are [private AWS fiber links interconnect all major regions][5].

Another thing I noticed is that globally speaking, **US East (N. Virginia) \| us-east-1** has the BEST average latency to the world in all 10 AWS regions. So if your start small and think big, `us-east-1` is probably the No.1 choice to start with.

[1]: https://gist.github.com/zhiguangwang/4b26feea87596a0ee676bc8b69dbb63f
[2]: https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#Images:visibility=public-images;search=099720109477/ubuntu/images/hvm-ssd/ubuntu-xenial-16.04-amd64-server-20160420.3;sort=desc:name
[3]: https://sa-east-1.console.aws.amazon.com/ec2/v2/home?region=sa-east-1#Images:visibility=public-images;search=ami-6008870c;sort=name
[4]: https://en.wikipedia.org/wiki/Kernel_panic
[5]: http://www.enterprisetech.com/2014/11/14/rare-peek-massive-scale-aws/
