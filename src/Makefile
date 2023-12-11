CC = gcc
FLAG_C = -c
FLAG_O = -o
# ASAN = -g -fsanitize=address
FLAG_COV = --coverage
FLAG_ER = -Wall -Werror -Wextra

all: clean gcov_report

s21_math.a:
	$(CC) $(FLAG_ER) $(FLAG_C) s21_math.c
	ar rc s21_math.a s21_math.o
	ranlib s21_math.a

ttt: s21_math.a
	$(CC) $(FLAG_ER) $(FLAG_C) tests.c main.c 
	ar rc suite.a tests.o
	ranlib suite.a
	$(CC) $(FLAG_ER) $(FLAG_COV) $(FLAG_O) ttt s21_math.a suite.a s21_math.c main.o tests.o -lcheck -lrt -lm -lsubunit -lpthread
	./ttt

gcov_report: ttt
	gcov s21_*.gcda	
	gcovr -b 
	gcovr
	gcovr --html-details -o report.html

check:
	cppcheck --enable=all --force *.h *.c
	cp ../materials/linters/CPPLINT.cfg CPPLINT.cfg
	python3 ../materials/linters/cpplint.py --extension=c *.c *.h
	CK_FORK=no leaks --atExit -- ./tests
	valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes --verbose --log-file=RESULT_VALGRIND.txt ./tests

clean:
	-rm -rf *.o *.html *.gcda *.gcno *.css *.a *.gcov *.info *.out *.cfg *.txt
	-rm -f ttt
	-rm -f report
	find . -type d -name 'tests.dSYM' -exec rm -r {} +