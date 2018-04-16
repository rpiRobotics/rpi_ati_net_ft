#!/usr/bin/env python

import rpi_ati_net_ft
import sys

def main():
    try:
        if (len(sys.argv) < 2):
            raise Exception('IP address of ATI Net F/T sensor required')
        host=sys.argv[1]
        netft=rpi_ati_net_ft.NET_FT(host)
        netft.set_tare_from_ft()
        print netft.read_ft_http()
        print netft.try_read_ft_http()
        
        netft.start_streaming()
        
        while(True):
            print netft.read_ft_streaming(.1)
            print netft.try_read_ft_streaming(.1)
        
    except KeyboardInterrupt:
        pass

if __name__ == '__main__':
    main()
