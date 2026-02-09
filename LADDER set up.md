# For MacOS 
  Installation of Anaconda3

    bash Anaconda3-2025.12-1-MacOSX-arm64.sh 
    source [path of anaconda is installed]/bin/activate
  
  Setting up python3.8.19

    conda create --name py3819 python=3.8.19              
    conda activate py3819
  Successful activation shall looks like "(py3819) [username]@[yourcomputer_name]"
  
###  important: all following installation shall be run in python3.8.19. higher version in Mac M2/M3 would fail
    
    pip install opencv-python==4.9.0.80    

  Then follow the rest setting up guide on : https://github.com/Mrwow/Ladder 
