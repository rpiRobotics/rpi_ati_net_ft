#!/usr/bin/env python

# Copyright (c) 2017, Rensselaer Polytechnic Institute, Wason Technology LLC
# All rights reserved.
# 
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met:
# 
#     * Redistributions of source code must retain the above copyright
#       notice, this list of conditions and the following disclaimer.
#     * Redistributions in binary form must reproduce the above copyright
#       notice, this list of conditions and the following disclaimer in the
#       documentation and/or other materials provided with the distribution.
#     * Neither the name of the Rensselaer Polytechnic Institute, or Wason 
#       Technology LLC, nor the names of its contributors may be used to 
#       endorse or promote products derived from this software without 
#       specific prior written permission.
# 
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
# AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
# ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE
# LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
# CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
# SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
# INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
# CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
# ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
# POSSIBILITY OF SUCH DAMAGE.


from geometry_msgs.msg import WrenchStamped
import rpi_ati_net_ft
import rospy

def main():
    
    rospy.init_node("ati_net_ft_driver")
    
    netft_host=rospy.get_param("~netft_host")
    publish_frequency=rospy.get_param("~publish_frequency", 50)
    
    
    ft_sensor = rpi_ati_net_ft.NET_FT(netft_host)
    ft_sensor.set_tare_from_ft()   
    ft_sensor.start_streaming()
    ft_wrench_pub = rospy.Publisher("ft_wrench", WrenchStamped, queue_size=10)
    
    rate = rospy.Rate(publish_frequency)
    
    while not rospy.is_shutdown():
        
        res, ft, status = ft_sensor.try_read_ft_streaming(0)
        
        if res and status == 0:
            w = WrenchStamped()
            w.header.stamp=rospy.Time.now()
            w.wrench.torque.x = ft[0]
            w.wrench.torque.y = ft[1]
            w.wrench.torque.z = ft[2]
            w.wrench.force.x = ft[3]
            w.wrench.force.y = ft[4]
            w.wrench.force.z = ft[5]
            
            ft_wrench_pub.publish(w)
                
        rate.sleep()
        
        
if __name__ == '__main__':
    main()
        
        
    
    
    



