# FreeRTOS_Task_Scheduling
This repository consists of an experiment which demonstrates the effect of time-slicing in FreeRTOS applications.
## Setup Guide:
- To setup this application, you need to first download:
  - [STM32CubeIDE](https://www.st.com/en/development-tools/stm32cubeide.html#st-get-software)
  - [STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html#st-get-software)
  - [SEGGER SystemView](https://www.segger.com/downloads/systemview/)

- Create a workspace folder where the project will reside.
- Clone the repository in the workspace folder. Incase of a `.zip` download, extract the repository to the workspace folder.
- For users who trying to setup this project on another **STM32** Controller, refer the following settings in the `FreeRTOS_Task_Scheduling.ioc` file:
  - Pinout & Configuration:
    - NVIC
    - SYS
    - RCC
  - Clock Configuration:
    - **NOTE:** Clock configurations can vary across MCUs. Make sure to go through your controller's data sheet for understanding the clock configuration and speeds.
  - Project Manager:
    - Project
    - Code Generator
- Launch **STM32CubeIDE** and select your workspace folder.
- Build the project by selecting `Project->Build Project` or right click on the project and select `Build Project`.

## Debugging Guide:
**NOTE:** The `.SVdat` files are already present in the repository. For those who just wish to see the trace can load the file directly onto the `SEGGER SystemView` application. If you are interested in exporting your own trace files, you can follow the debugging guide.

Connect your STM32 board to your PC/Laptop and click on the following settings:
- Select `Run->Debug Configurations`.
    
  - <img width="1454" height="927" alt="Screenshot (4)" src="https://github.com/user-attachments/assets/ba0601c2-de43-4212-be1c-0950aa98a8a8" />
- Ensure that `SWD` is selected as your target interface and `SWO` is enabled. Enter the `Core Clock (MHZ)` value as per your MCU's core clock configuration in the `FreeRTOS_Task_Scheduling.ioc` file.
- Once configured, click on Apply and then Debug.
- The IDE will then shift onto a debug window, click on `Window->Show View->Expressions`, `Window->Show View->Memory Browser` and `Window->Show View->Other->SWV->SWV ITM Data Console`.
- In the Expressions tab, add a new expression named `_SEGGER_RTT` and monitor the `_SEGGER_RTT->aUp[1]->pBuffer` variable.
  -  <img width="657" height="758" alt="Screenshot (5)" src="https://github.com/user-attachments/assets/15f2526c-d5ab-4eac-8229-7ce370e22dff" />
- Before starting the execution of the program, head to the `SWV ITM Data Console` tab and click on the `Configure trace` icon.
  -   <img width="1179" height="696" alt="Screenshot (6)" src="https://github.com/user-attachments/assets/b27b386b-2c0b-422c-8b8f-c6f2889ecab4" />
- Make sure to enable port 0 int the `ITM Stimulus Ports` option.
- Click `OK` and select the `Start Trace` icon.
- Resume the execution of the program and you will see data displayed on the `SWV ITM Data Console`.
  -   <img width="1921" height="623" alt="Screenshot (3)" src="https://github.com/user-attachments/assets/d35a577f-01ac-448d-9c7e-3365812fdda3" />
- Let the program run for a few seconds, then halt the execution and copy the address stored in `_SEGGER_RTT->aUp[1]->pBuffer`.
  - <img width="1033" height="821" alt="Screenshot (7)" src="https://github.com/user-attachments/assets/fed5a05b-f0e4-4509-9105-684244a1da1c" />
- Head to the `Memory Browser` tab and paste the address in the address space and click `Go`
  - <img width="1914" height="612" alt="Screenshot (8)" src="https://github.com/user-attachments/assets/8668a13f-e33f-49e4-b814-a24105c70efe" />
- Click on the `Export` icon above the `Go` button and provide a path where you want to store the `.SVdat` file. In the `Length` tab enter `4096` and click `OK`.
  - <img width="748" height="266" alt="Screenshot (9)" src="https://github.com/user-attachments/assets/56afeae3-b19e-4788-a672-53ca010094cc" />

## Trace Analysis Guide:
- Open `SEGGER SystemView` on your PC/Laptop.
- Click on `File->Load Recording` and select the `.SVdat` file exported.
- Click `OK` in the `System Information` tab and you will see a window like this:
  - <img width="2560" height="1440" alt="Screenshot (10)" src="https://github.com/user-attachments/assets/73aee306-1c0d-458c-acbb-bbbd88d4928a" />
- You can now view the trace of the task execution and also scroll along the `timeline` tab and see how each task execution takes place and other task metrics.



