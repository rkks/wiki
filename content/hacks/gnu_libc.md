% GNU libc facilities for debugging
% Ravikiran K.S.
% January 1, 2006
Runtime Inspection Tools

GNU C library provides some very useful calls which can be turned on or used in
code for runtime debugging of the code. Few important calls are described below:

1. ptrace() -

2. mtrace() -

3. gcov - GNU Code Coverage Tool is mostly used to check all the code paths that
are hit and the ones that are never touched.

[rkks@in-fuchsia][~/test]$ gcov test_sw_num.c
test_sw_num.gcno:cannot open graph file

[rkks@in-fuchsia][~/test]$ gcc -fprofile-arcs -ftest-coverage -lgcov -o gcov-test test_sw_num.c
test_sw_num.c: In function `l2ha_switch_id':
test_sw_num.c:26: warning: initialization makes pointer from integer without a cast
[rkks@in-fuchsia][~/test]$ ./gcov-test
Global switch id is: 0
Damon : Opened socket , mac 00:52:BB:40:06:41
Damon : Opened socket , mac 00:52:BB:40:06:44

[rkks@in-fuchsia][~/test]$ gcov test_sw_num.gcno
File `test_sw_num.c'
Lines executed:94.44% of 18
test_sw_num.c:creating `test_sw_num.c.gcov'

[rkks@in-fuchsia][~/test]$ gcov test_sw_num.gcda
File `test_sw_num.c'
Lines executed:94.44% of 18
test_sw_num.c:creating `test_sw_num.c.gcov'

[rkks@in-fuchsia][~/test]$ cat test_sw_num.c.gcov
        -:    0:Source:test_sw_num.c
        -:    0:Graph:test_sw_num.gcno
        -:    0:Data:test_sw_num.gcda
        -:    0:Runs:1
        -:    0:Programs:1
        -:    1:/*
        -:    2: * =====================================================================================
        -:    3: *
        -:    4: *       Filename:  test_sw_num.c
        -:    5: *
        -:    6: *    Description:  Test FM_SW_NUM variable.
        -:    7: *
        -:    8: *        Version:  1.0
        -:    9: *        Created:  05/05/09 14:05:26
        -:   10: *       Revision:  none
        -:   11: *       Compiler:  gcc
        -:   12: *
        -:   13: *         Author:  Ravikiran K.S. (rkks), mr.rkks@gmail.com
        -:   14: *        Company:  Continuous Computing Corporation (www.ccpu.com)
        -:   15: *
        -:   16: * =====================================================================================
        -:   17: */
        -:   18:
        -:   19:#include <unistd.h>
        -:   20:
        -:   21:int g_sw = 0;
        -:   22:
        -:   23:static int
        -:   24:l2ha_switch_id()
function l2ha_switch_id called 1 returned 100% blocks executed 67%
        1:   25:{
        1:   26:        char *id = getenv("FM_SW_NUM");
        -:   27:
        1:   28:        if (!id)
        1:   29:                return 0;
        -:   30:        else
    #####:   31:                return(atoi(id));
        -:   32:}
        -:   33:
        -:   34:#define MAX_MAC 26
        -:   35:
        -:   36:int
        -:   37:get_sys_mac()
function get_sys_mac called 1 returned 100% blocks executed 100%
        1:   38:{
        -:   39:/*   int               s;*/
        -:   40:/*   struct ifreq      ifr;*/
        -:   41:/*   int                                        rc;*/
        1:   42:        unsigned char                                   eth0_mac[6]={0x00, 0x52, 0xBB, 0x40, 0x06, 0x41};
        -:   43:
        1:   44:        printf("Damon : Opened socket , mac %02X:%02X:%02X:%02X:%02X:%02X\n",
        -:   45:                eth0_mac[0],
        -:   46:                eth0_mac[1],
        -:   47:                eth0_mac[2],
        -:   48:                eth0_mac[3],
        -:   49:                eth0_mac[4],
        -:   50:                eth0_mac[5]);
        -:   51:
        -:   52:
        -:   53:/*   if ((s = socket(AF_INET, SOCK_DGRAM, IPPROTO_IP)) < 0)*/
        -:   54:/*      return 0;*/
        -:   55:
        -:   56:/*   strncpy(ifr.ifr_name, IFNAME, sizeof(ifr.ifr_name));*/
        -:   57:
        -:   58:/*   rc = ioctl(s, SIOCGIFHWADDR, &ifr);*/
        -:   59:/*   if (rc == 0) {*/
        -:   60:/*      eth0_mac = (fm_byte *)&(ifr.ifr_hwaddr.sa_data);*/
        1:   61:                eth0_mac[5] += 3;
        1:   62:                eth0_mac[5] += (g_sw * MAX_MAC);
        -:   63:/*      memcpy(mac, &eth0_mac[0], ETH_ALEN);*/
        -:   64:/*   }*/
        1:   65:        printf("Damon : Opened socket , mac %02X:%02X:%02X:%02X:%02X:%02X\n",
        -:   66:                eth0_mac[0],
        -:   67:                eth0_mac[1],
        -:   68:                eth0_mac[2],
        -:   69:                eth0_mac[3],
        -:   70:                eth0_mac[4],
        -:   71:                eth0_mac[5]);
        -:   72:
        -:   73:/*   close(s);*/
        1:   74:        return(0);
        -:   75:}
        -:   76:
        -:   77:int main(int argc, char **argv)
function main called 1 returned 100% blocks executed 100%
        1:   78:{
        1:   79:        g_sw = l2ha_switch_id();
        1:   80:        printf("Global switch id is: %d\n", g_sw);
        1:   81:        sleep(1);
        1:   82:        get_sys_mac();
        1:   83:        return 0;
        -:   84:}
        -:   85:

