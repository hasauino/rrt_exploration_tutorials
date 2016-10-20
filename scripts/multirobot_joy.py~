#!/usr/bin/env python


#--------Include modules---------------
import rospy
from sensor_msgs.msg import Joy
from geometry_msgs.msg import Twist

#-----------------------------------------------------
# Subscribers' callbacks------------------------------


def callBack(data):
    global joyData,max_rot,max_lin,n_robots,vel_msg
    joyData=data
    
    #axes=data.axes
    #buttons=data.buttons
    
    
    
    vel_msg.linear.x=data.axes[1]*max_lin*(data.axes[3]+1.0)*0.5
    vel_msg.linear.y=0.0
    vel_msg.linear.z=0.0
    
    vel_msg.angular.x=0.0
    vel_msg.angular.y=0.0
    vel_msg.angular.z=data.axes[2]*max_rot*(data.axes[3]+1.0)*0.5
    



# Node----------------------------------------------
def node():

	global joyData,max_rot,max_lin,n_robots,vel_msg
	vel_msg=Twist()
	joyData=Joy()
	rospy.init_node('multirobot_joy', anonymous=False)
	
	# fetching all parameters
	n_robots = rospy.get_param('~n_robots',3)
	max_rot= rospy.get_param('~max_rot',6.0)
	max_lin= rospy.get_param('~max_lin',1.5)	
	
	
#-------------------------------------------
    	rospy.Subscriber("/joy", Joy, callBack)

	rate = rospy.Rate(100)
	while joyData.header.seq<1:
		pass	
	
	
	pubs=list()
    	for i in range(0,n_robots):
		pubs.append( rospy.Publisher('/robot_'+str(i+1)+'/mobile_base/commands/velocity', Twist, queue_size=10))		#/keyop_vel_smoother/raw_cmd_vel	/mobile_base/commands/velocity
		


    	zero=Twist()
    	vel_msg.linear.x=0.0
	vel_msg.linear.y=0.0
	vel_msg.linear.z=0.0
    
	vel_msg.angular.x=0.0
	vel_msg.angular.y=0.0
	vel_msg.angular.z=0.0
    	
    
#-------------------------------------------------------------------------
	while not rospy.is_shutdown():

	  for i in range(0,n_robots):
	  
	  	if joyData.buttons[6+i]>0.0: 
	      	  	pubs[i].publish(vel_msg)
	      	else:
	      		pubs[i].publish(zero)
	      	
    	  




#_____________________________________________________________________________

if __name__ == '__main__':
    try:
        node()
    except rospy.ROSInterruptException:
        pass
 
 
 
 
