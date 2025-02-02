import math
from haversine import haversine,Unit

#Function for reading file 1
def readfile_1():
 with open("File_1.txt","r") as File_1:
    text1=File_1.read()
 return text1

#Function for reading file 2
def readfile_2():
 with open("File_2.txt","r") as File_2:
    text2=File_2.read() 
 return text2

#Function for calculating range
def calc_range(vel_res,height):
  g=9.8 
  range=float(vel_res*(math.sqrt(2*height/g)))
  return(range)

#Function for calculating distance b/w drop location and current position
def calc_dist(lat_p,lon_p,lat_target,lon_target):
  location_1=(lat_p,lon_p)
  location_2=(lat_target,lon_target)
  drop_dist=haversine(location_1,location_2,'m')
  return(drop_dist)

#Function for planning trajectory
def msg():
  print("Drop location out of range")
  print("  Traversing towards it...")
#Function for ejection
def eject():
  print("Target in range")
  print()
  print("Ejecting payload...")
  print("Payload ejected")  

wind_velocity=0

#Read File_1 for drone position and velocity
text1=readfile_1()
lat_p=float(text1.split("lat_p")[1].split("lon_p")[0].strip())  #Drone position coordinates
lon_p=float(text1.split("lon_p")[1].split("vel_x")[0].strip())

vel_x=float(text1.split("vel_x")[1].split("vel_y")[0].strip())   #Drone velocity
vel_y=float(text1.split("vel_y")[1].strip())

vel_res=(math.sqrt(vel_x**2+vel_y**2)) + wind_velocity   #Resultant velocity

height=20  #Height of Drone from the ground


#Read File_2 for target coordinates
text2=readfile_2()
lat_target=float(text2.split("lat_target")[1].split("lon_target")[0].strip())  #Target coordinates
lon_target=float(text2.split("lon_target")[1].strip())


range=calc_range(vel_res,height)  #calculating range

drop_dist=calc_dist(lat_p,lon_p,lat_target,lon_target)   #calculating drop distance

print("Target distance:",drop_dist,'m',"     Range:",range,'m')



if(range<drop_dist-1):  
  msg() 
  while(range<drop_dist-1):   #While drop location is out of range 
 
    text1=readfile_1()          #Storing data from files to variables  
 
    lat_p=text1.split("lat_p")[1].split("lon_p")[0].strip()  #Drone position coordinates
    lat_p=float(lat_p[0]) 

    lon_p=text1.split("lon_p")[1].split("vel_x")[0].strip()
    lon_p=float(lon_p[0])

    vel_x=text1.split("vel_x")[1].split("vel_y")[0].strip()   #Drone velocity
    vel_x=float(vel_x[0])

    vel_y=text1.split("vel_y")[1].strip()
    vel_y=float(vel_y[0])

    vel_res=math.sqrt(vel_x**2+vel_y**2)  #Resultant velocity

    height=20  #Height of Drone from the ground

    text2=readfile_2()
    lat_target=text2.split("lat_target")[1].split("lon_target")[0].strip()  #Target coordinates
    lat_target=float(lat_target[0]) 

    lon_target=text2.split("lon_target")[1].strip()
    lon_target=float(lon_target[0])

    range=calc_range(vel_res,height)  #calculating range
 
    drop_dist=calc_dist(lat_p,lon_p,lat_target,lon_target)   #calculating drop distance

if(drop_dist-1<=range<=drop_dist+1): 
 eject()                         #Ejecting payload
 print("  Mission completed!!") 
