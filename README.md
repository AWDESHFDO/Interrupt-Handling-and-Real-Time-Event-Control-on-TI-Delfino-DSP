In this experiment, I programmed the TI TMS320F28379D Delfino DSP to handle both external and internal interrupts using Code Composer Studio (CCS12). The goal was to understand interrupt configuration, service routines, and interrupt prioritization between asynchronous external signals and timer-based internal events.

In Task 1, I configured an external interrupt (XINT1) triggered by push button PB2, which is connected to GPIO17. I initialized the PIE Vector Table using InitPieCtrl() and InitPieVectTable() and redirected the XINT1 interrupt to a custom Interrupt Service Routine (ISR) named xint1_isr. The input qualification and sampling rate for GPIO17 were configured using GPAQSEL2 and GPACTRL registers, and the signal was inverted using GPAINV to accommodate the active-low logic of PB2.

The XINT1 interrupt was configured to trigger on the rising edge and enabled through PieCtrlRegs and XintRegs. Inside the ISR, I toggled GPIO34 to drive LED LD3, visually confirming the interrupt response. After servicing the interrupt, I manually cleared the PIEACK bit to allow future interrupts from the same group.

In Task 2, I implemented an internal interrupt using CPU Timer0. The timer was configured to generate an interrupt every 8 seconds. I redirected the PIE vector for TIMER0_INT to a new ISR called cpu_timer0_isr. Inside this ISR, I controlled GPIO41 to drive LED LD10, turning it ON for 3 seconds before switching it OFF.

When both interrupts were active, I observed that while the Timer0 interrupt was being serviced (LD10 ON), the external interrupt (PB2) did not immediately toggle LD3. However, once the Timer0 ISR completed, LD3 toggled instantly, showing that the external interrupt flag had been set but could not preempt the higher-priority Timer0 task.

This experiment demonstrated interrupt nesting, priority handling, and synchronization between CPU tasks. It provided practical insight into how DSPs manage real-time event-driven control through the PIE (Peripheral Interrupt Expansion) module, global interrupt enabling, and proper ISR design.
