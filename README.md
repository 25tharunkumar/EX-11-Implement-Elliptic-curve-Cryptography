# EX-11-Implement-Elliptic-curve-Cryptography

## AIM:

To implement the Elliptic curve Cryptography

## ALGORITHM:

### STEP-1:

Alice and Bob agree on the elliptic curve equation, base point G, and prime modulus p.

### Step 2:

Alice selects a random private key a and computes her public key A =a-G

### Step 3:

Alice sends her public key A to Bob.

### Step 4:

Bob selects a random private key b and computes his public key B=b-G.

### Step 5:

Bob sends his public key B to Alice.

### Step 6:

Alice computes the shared secret key as Secret=a-B. Bob computes the shared secret key as Secret-b-A.

## PROGRAM:

#include <stdio.h>

struct Point {
    long long x;
    long long y;
};

// Function to calculate the modular inverse
long long mod_inverse(long long a, long long p) {
    long long m = p, t, q;
    long long x = 0, y = 1;
    if (p == 1) return 0;

    while (a > 1) {
        q = a / p;
        t = p;
        p = a % p;
        a = t;
        t = x;
        x = y - q * x;
        y = t;
    }
    if (y < 0) y += m;

    return y;
}

// Function for point addition
struct Point point_addition(struct Point P, struct Point Q, long long a, long long p) {
    struct Point R;

    if (P.x == Q.x && P.y == Q.y) {
        long long s = (3 * P.x * P.x + a) * mod_inverse(2 * P.y, p) % p;
        R.x = (s * s - 2 * P.x) % p;
        R.y = (s * (P.x - R.x) - P.y) % p;
    } else {
        long long s = (Q.y - P.y) * mod_inverse(Q.x - P.x, p) % p;
        R.x = (s * s - P.x - Q.x) % p;
        R.y = (s * (P.x - R.x) - P.y) % p;
    }

    if (R.x < 0) R.x += p;
    if (R.y < 0) R.y += p;

    return R;
}

// Function for scalar multiplication
struct Point scalar_multiplication(struct Point P, long long k, long long a, long long p) {
    struct Point R = P;
    k = k - 1;

    while (k > 0) {
        if (k % 2 == 1) {
            R = point_addition(R, P, a, p);
        }
        P = point_addition(P, P, a, p);
        k /= 2;
    }

    return R;
}

int main() {
    long long a, b, p;
    struct Point G;
    long long private_key;
    struct Point public_key;

    printf("Enter the value of 'a' for the elliptic curve equation y^2 = x^3 + ax + b (mod p): ");
    scanf("%lld", &a);

    printf("Enter the value of 'b' for the elliptic curve equation y^2 = x^3 + ax + b (mod p): ");
    scanf("%lld", &b);

    printf("Enter the prime number 'p' for the finite field (modulus): ");
    scanf("%lld", &p);

    printf("Enter the x-coordinate of the base point G: ");
    scanf("%lld", &G.x);

    printf("Enter the y-coordinate of the base point G: ");
    scanf("%lld", &G.y);

    printf("Enter the private key: ");
    scanf("%lld", &private_key);

    public_key = scalar_multiplication(G, private_key, a, p);

    printf("\nElliptic Curve: y^2 = x^3 + %lldx + %lld (mod %lld)\n", a, b, p);
    printf("Base Point G: (%lld, %lld)\n", G.x, G.y);
    printf("Private Key: %lld\n", private_key);
    printf("Public Key: (%lld, %lld)\n", public_key.x, public_key.y);

    return 0;
}

## OUTPUT:
![Screenshot 2024-10-30 084501](https://github.com/user-attachments/assets/065631d7-ec6a-48e2-987e-2bb1cd3bf3d9)

## RESULT:
Thus Implement-Elliptic-curve-Cryptography has been successfully implemented using c
