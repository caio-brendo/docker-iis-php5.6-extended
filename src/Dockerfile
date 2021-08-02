# Dockerfile for Windows Servercore + PHP 5.6 + Apache 2.4
# This Dockerfile install some dll´s to connect with sql server and xdebug connect

FROM caiobrendo/iis-php5.6

MAINTAINER caioaraujo.immv@gmail.com

# Download section
ADD https://github.com/git-for-windows/git/releases/download/v2.31.1.windows.1/Git-2.31.1-64-bit.exe Git-2.31.1-64-bit.exe
ADD 'https://drive.google.com/uc?export=download&id=1JBDL28Y7N-CvRMZyewGjf5aS5FcqPCsf' composer.zip
ADD 'https://drive.google.com/uc?export=download&id=10482rur9uH85cLmZ0L2hApnq-jkDPucD' php.zip
ADD 'https://drive.google.com/uc?export=download&id=1vVzxtbtAVGRge9gm4fVdWfbSdSevjk3p' modules-iis.zip
ADD 'https://drive.google.com/uc?export=download&id=1PV0MXSzcmla0rsF0rvKLP6f23j_AlBIL' applicationhost.zip
# End section

# Unzip archives
RUN powershell -command Expand-Archive -Path C:\composer.zip -DestinationPath C:\
RUN powershell -Command Expand-Archive -Path C:\php.zip -DestinationPath C:\\ -Force
RUN powershell -Command Expand-Archive -Path C:\modules-iis.zip -DestinationPath C:\modules-iis

# Set environment variables
RUN setx PATH /M %PATH%;C:\composer\bin
RUN setx XDEBUG_CONFIG "client_host=192.168.0.110"
RUN setx PHP_IDE_CONFIG "serverName=xdebug-docker"

#Install

RUN C:\Git-2.31.1-64-bit.exe /VERYSILENT /NORESTART
RUN msiexec /i modules-iis\requestRouter_amd64.msi /quiet /qn /passive /L*v "C:\msodbcsq2.log"
RUN msiexec /i modules-iis\rewrite_amd64_en-US.msi /quiet /qn /passive /L*v "C:\re.log"

# Create dir´s
RUN mkdir tmp


# Set custom data config
RUN powershell -Command Expand-Archive -Path C:\applicationhost.zip -DestinationPath C:\Windows\System32\inetsrv\config -Force

# Delete downloads archives
RUN powershell -Command Remove-Item php.zip,Git-2.31.1-64-bit.exe,composer.zip,modules-iis.zip,applicationhost.zip -Force