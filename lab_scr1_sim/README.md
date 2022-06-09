# Постановка задачи

Обработать исключение Environment call form M-mode выводом строки "envcall". Настроить ресет вектор и вектор обработки прерываний на 0xB000 и 0xA880 соответственно. Проверить работу программы на примере isa/rv32mi/scall.S.

| Номер варианта  | Вид исключения | Тест | Reset Vector | Trap Vector | Обработчик |
| --- | --- | --- | --- | --- |  --- |
| 6 | Environment call form M-mode | isa/rv32mi/scall.S | 0xB000 | 0xA880 | Вывод строки «envcall» |

# Ход работы

1. Создан рабочий репозиторий - fork от репозитория SCR1 https://github.com/syntacore/scr1

---

2. До внесения изменений проект запускается.

---

3.  Отредактирован список тестов, чтобы на выполнение остался только тест по варианту задания.


В файле со списком тестов *rv32_tests.inc* оставлен только тест *scall.S*.
Заданы переменные PATH.
```
make clean
make TARGETS="riscv_isa"
```



---
4. Модифицирована обработку исключений trap_vector в файле ./sim/tests/common/*riscv_macros.h*
5. В файле ./src/includes/*scr1_arch_description.svh* параметры ядра *Reset Vector* и *Trap Vector* установлены в соответствии с вариантом задания (0xB000, 0xA880).
6. Изменен linker-скрипт ./sim/tests/common/*link.ld*

Вывод консоли:
```
---Test:                        scall.hex
Test passed

#--------------------------------------
# Summary: 1/1 tests passed
#--------------------------------------

- /home/xuchaku/Desktop/task2lab/scr1/src/tb/scr1_top_tb_runtests.sv:181: Verilog $finish
Simulation performed on Verilator 4.220 2022-03-12 rev v4.220 
                          Test               | build | simulation 
                       scall.hex		OK	  PASS 

                    
```
Также в dump-файле *scall.dump* адреса при `<_start>` и `<trap_vector>` соответствуют указанным в задании *Reset Vector* и *Trap Vector* соответственно.
7. Также была запущена команда 
```
make run_verilator_wf TARGETS="riscv_isa" TRACE=1.
```
с помощью которой был сгененрирован wave form.

Папка results содержит следующие файлы scall.dump, sim_results.txt, test_results.txt, tracelog_core_0.log
