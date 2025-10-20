#include <iostream>
#include <vector>
#include <cmath>
#include <ctime>
#include <random>
#include <iomanip>
#include <windows.h>
using namespace std;

float* myRand(int n)
{
    float* r = new float[n];
    long long y;
    static long long b = time(NULL);
    for (int i = 0; i < n; i++)
    {
        y = b * 1220703125LL;
        y = y % (1LL << 31);
        if (y < 0) y = -y;
        r[i] = (float)y / (float)(1LL << 31);
        b = y;
    }
    return r;
}

// библиотечный генератор
float* libRand(int n)
{
    float* r = new float[n];
    mt19937 gen(time(NULL));
    uniform_real_distribution<float> dist(0.0, 1.0);
    for (int i = 0; i < n; i++)
        r[i] = dist(gen);
    return r;
}

// проверка равномерности (через chi2)
void checkUniform(float* arr, int n, int k)
{
    vector<int> cnt(k, 0);
    for (int i = 0; i < n; i++)
    {
        int idx = int(arr[i] * k);
        if (idx == k) idx = k - 1;
        cnt[idx]++;
    }

    double expected = (double)n / k;
    double chi2 = 0;
    for (int i = 0; i < k; i++)
    {
        double diff = cnt[i] - expected;
        chi2 += diff * diff / expected;
    }
    cout << "chi2 = " << fixed << setprecision(3) << chi2 << " (проверка равномерности)\n";
}

// проверка автокорреляции 
void checkAutoCorr(float* arr, int n)
{
    double mean = 0;
    for (int i = 0; i < n; i++) mean += arr[i];
    mean /= n;

    double num = 0, den = 0;
    for (int i = 0; i < n - 1; i++)
        num += (arr[i] - mean) * (arr[i + 1] - mean);
    for (int i = 0; i < n; i++)
        den += (arr[i] - mean) * (arr[i] - mean);

    double r1 = num / den;
    cout << "Автокорреляция: " << fixed << setprecision(5) << r1 << "\n\n";
}

int main()
{
    setlocale(LC_ALL, "Russian");
    int n = 100000; // сколько чисел генерим
    int k = 10;     // сколько корзин для chi2

    cout << "Лабораторная по ГПСЧ\n";
    cout << "Генерируем " << n << " чисел и проверяем качество генераторов\n\n";

    cout << "== Мой генератор ==\n";
    float* arr1 = myRand(n);
    checkUniform(arr1, n, k);
    checkAutoCorr(arr1, n);
    delete[] arr1;

    cout << "== std::mt19937 ==\n";
    float* arr2 = libRand(n);
    checkUniform(arr2, n, k);
    checkAutoCorr(arr2, n);
    delete[] arr2;

    return 0;
}
