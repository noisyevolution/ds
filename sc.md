# Soft Computing — Practicals

---

## Table of Contents

1. [Practical 1](#practical-1) — Linear neural network, Binary & Bipolar sigmoidal
2. [Practical 2](#practical-2) — McCulloch-Pitts: AND/NOT, XOR
3. [Practical 3](#practical-3) — Hebb's rule, Delta rule
4. [Practical 4](#practical-4) — Back Propagation Algorithm
5. [Practical 5](#practical-5) — Hopfield Network, Radial Basis Function
6. [Practical 6](#practical-6) — Kohonen Self-Organizing Map
7. [Practical 7](#practical-7) — Linear separation (Perceptron)
8. [Practical 8](#practical-8) — Membership & Identity operators
9. [Practical 9](#practical-9) — Fuzzy logic: ratios, Tipping problem

---

# Practical 1

## Design a simple linear neural network model

```python
x1 = float(input("X1: "))
x2 = float(input("X2: "))
w1 = float(input("W1: "))
w2 = float(input("W2: "))

yin = x1*w1 + x2*w2
print("Net Input =", yin)

yout = 1 if yin > 0 else 0
print("Output =", yout)
```
## Neural net using both binary and bipolar sigmoidal function

```python
import numpy as np

x1 = float(input("X1: "))
x2 = float(input("X2: "))
w1 = float(input("W1: "))
w2 = float(input("W2: "))

yin = 1 + (x1*w1 + x2*w2)
print("Net Input =", yin)

bin_out = 1 / (1 + np.exp(-yin))
bip_out = 2/(1 + np.exp(-yin)) - 1

print("Binary Output =", round(bin_out, 4))
print("Bipolar Output =", round(bip_out, 4))

```

---

# Practical 2

## Generate AND/NOT function using McCulloch Pitts neural net

```python
def neuron(inputs, weights, th):
    return 1 if sum(i*w for i, w in zip(inputs, weights)) >= th else 0

def AND(x1, x2):
    return neuron([x1, x2], [1, 1], 2)

def NOT(x):
    return neuron([x], [-1], 0)

print("AND:", AND(0,0), AND(0,1), AND(1,0), AND(1,1))
print("NOT:", NOT(0), NOT(1))

```
## Generate XOR function using McCulloch-Pitts neural net
```python
import numpy as np

def sigmoid(x):
    return 1 / (1 + np.exp(-x))

# Network params
w_in_h = np.array([[20, -20],
                   [20, -20]])
w_h_out = np.array([[20],
                    [20]])
b_h = np.array([[-10, 30]])
b_o = np.array([[-30]])

def xor(x1, x2):
    x = np.array([[x1, x2]])
    h = sigmoid(x @ w_in_h + b_h)
    o = sigmoid(h @ w_h_out + b_o)
    return o[0, 0]

print("XOR(0,0) =", xor(0,0))
print("XOR(0,1) =", xor(0,1))
print("XOR(1,0) =", xor(1,0))
print("XOR(1,1) =", xor(1,1))

```

---

# Practical 3

## Write a program to implement Hebb's rule

```python
w1 = w2 = b = 0
x1 = [-1, -1,  1, 1]
x2 = [-1,  1, -1, 1]
y  = [-1,  1,  1, 1]

def train(x1, x2, y):
    global w1, w2, b
    for i in range(len(x1)):
        w1 += x1[i] * y[i]
        w2 += x2[i] * y[i]
        b  += y[i]
        print(f"Epoch {i+1}: w1={w1}, w2={w2}, b={b}")

print("Training (OR examples):")
train(x1, x2, y)

print("\nFinal Weights:")
print("w1 =", w1)
print("w2 =", w2)
print("b  =", b)

```

## Write a program to implement the Delta rule

```python
import numpy as np

X = np.array([[0,0],[0,1],[1,0],[1,1]])
Y = np.array([0,1,1,1])

weights = np.random.rand(2)
bias = np.random.rand(1)
lr = 0.1
epochs = 200

sigmoid = lambda x: 1/(1+np.exp(-x))

def train(X, Y, w, b, lr, epochs):
    for e in range(epochs):
        for i in range(len(X)):
            out = sigmoid(np.dot(X[i], w) + b)
            err = Y[i] - out
            w += lr * err * X[i]
            b += lr * err
        if e % 100 == 0:
            te = np.mean((Y - sigmoid(X @ w + b))**2)
            print(f"Epoch {e}: Error {te}")
    return w, b

w, b = train(X, Y, weights, bias, lr, epochs)

print("\nFinal Weights:")
print("w1 =", round(w[0], 2))
print("w2 =", round(w[1], 2))
print("b  =", round(b[0], 2))

```

---

# Practical 4

## Write a program for Back Propagation Algorithm

```python
import numpy as np

sigmoid = lambda x: 1/(1+np.exp(-x))
sig_der = lambda x: x*(1-x)

class NN:
    def __init__(self, inp, hid, out):
        self.w1 = np.random.uniform(-1, 1, (inp, hid))
        self.w2 = np.random.uniform(-1, 1, (hid, out))

    def forward(self, x):
        self.h = sigmoid(x @ self.w1)
        self.o = sigmoid(self.h @ self.w2)
        return self.o

    def backward(self, x, y, lr):
        e  = y - self.o
        d2 = e * sig_der(self.o)
        d1 = (d2 @ self.w2.T) * sig_der(self.h)

        self.w2 += np.outer(self.h, d2) * lr
        self.w1 += np.outer(x, d1) * lr

    def train(self, X, Y, epochs, lr):
        for _ in range(epochs):
            for i in range(len(X)):
                self.forward(X[i])
                self.backward(X[i], Y[i], lr)

    def predict(self, x):
        return self.forward(x)


# XOR data
X = np.array([[0,0],[0,1],[1,0],[1,1]])
Y = np.array([[0],[1],[1],[0]])

nn = NN(2, 4, 1)
nn.train(X, Y, epochs=10000, lr=0.1)

for x in X:
    print("Input:", x, "Output:", nn.predict(x))

```

---

# Practical 5

## Write a program for Hopfield Network

```python
import numpy as np

class Hopfield:
    def __init__(self, n):
        self.n = n
        self.w = np.zeros((n, n))

    def train(self, patterns):
        for p in patterns:
            p = p.reshape(self.n, 1)
            self.w += np.outer(p, p)
        np.fill_diagonal(self.w, 0)

    def predict(self, x, max_iter=100):
        x = x.reshape(self.n, 1)
        for _ in range(max_iter):
            y = np.sign(self.w @ x)
            if np.array_equal(x, y):
                return y.flatten()
            x = y
        raise RuntimeError("Did not converge")

# Training patterns
p1 = np.array([ 1, -1,  1, -1])
p2 = np.array([-1, -1, -1,  1])
patterns = [p1, p2]

net = Hopfield(len(p1))
net.train(patterns)

noisy = np.array([1, 1, 1, 1])
pred = net.predict(noisy)

print("Noisy :", noisy)
print("Output:", pred)

```

## Write a program for Radial Basis function

```python
import numpy as np
from sklearn.cluster import KMeans
from sklearn.datasets import make_classification
from sklearn.metrics import accuracy_score
import matplotlib.pyplot as plt

# Dataset
X, y = make_classification(
    n_samples=200, n_features=2, n_classes=2,
    n_clusters_per_class=1, n_informative=2,
    n_redundant=0, random_state=42
)

plt.scatter(X[:,0], X[:,1], c=y, cmap='viridis')
plt.title("Dataset")
plt.show()

# -------- RBF Network --------
class RBF:
    def __init__(self, k):
        self.k = k

    def _rbf(self, x, c, s):
        return np.exp(-np.linalg.norm(x-c)**2 / (2*s**2))

    def _A(self, X):
        A = np.zeros((len(X), self.k))
        for i, c in enumerate(self.centers):
            A[:, i] = [self._rbf(x, c, self.sigmas[i]) for x in X]
        return A

    def fit(self, X, y):
        km = KMeans(n_clusters=self.k, random_state=42).fit(X)
        self.centers = km.cluster_centers_

        d = np.max([np.linalg.norm(a-b) for a in self.centers for b in self.centers])
        self.sigmas = np.full(self.k, d / np.sqrt(2*self.k))

        A = self._A(X)
        self.weights = np.linalg.pinv(A) @ y

    def predict(self, X):
        A = self._A(X)
        return np.round(A @ self.weights).astype(int)

# Train
rbf = RBF(k=10)
rbf.fit(X, y)

# Predict
y_pred = rbf.predict(X)
print("Accuracy:", round(accuracy_score(y, y_pred)*100, 2), "%")

# Decision boundary
x_min, x_max = X[:,0].min()-1, X[:,0].max()+1
y_min, y_max = X[:,1].min()-1, X[:,1].max()+1
xx, yy = np.meshgrid(np.arange(x_min, x_max, 0.1),
                     np.arange(y_min, y_max, 0.1))

Z = rbf.predict(np.c_[xx.ravel(), yy.ravel()]).reshape(xx.shape)

plt.contourf(xx, yy, Z, alpha=0.3, cmap='viridis')
plt.scatter(X[:,0], X[:,1], c=y, edgecolors='k', cmap='viridis')
plt.title("RBF Decision Boundary")
plt.show()

```

---

# Practical 6

## Kohonen Self-Organizing Map

```python
import numpy as np
import matplotlib.pyplot as plt

# Params
grid = (10, 10)
dim = 3
lr = 0.5
iters = 1000
decay = 0.1
radius = 3

data = np.random.rand(300, dim)
som = np.random.rand(grid[0], grid[1], dim)

def bmu(x, som):
    d = np.linalg.norm(som - x, axis=2)
    return np.unravel_index(np.argmin(d), d.shape)

def update(som, x, b, lr, r):
    for i in range(som.shape[0]):
        for j in range(som.shape[1]):
            d = np.sqrt((i-b[0])**2 + (j-b[1])**2)
            if d <= r:
                inf = np.exp(-d*d/(2*r*r))
                som[i,j] += inf * lr * (x - som[i,j])

for _ in range(iters):
    x = data[np.random.randint(len(data))]
    b = bmu(x, som)
    update(som, x, b, lr, radius)
    lr *= (1 - decay)
    radius *= (1 - decay)

plt.imshow(som)
plt.title("Kohonen SOM")
plt.show()

```

---

# Practical 7

## Write a program for Linear separation

```python
import numpy as np
import matplotlib.pyplot as plt

def plot_data(X, y, w):
    plt.figure(figsize=(8,6))

    for i in range(len(X)):
        c = 'blue' if y[i] == 1 else 'red'
        m = 'o' if y[i] == 1 else 'x'
        plt.scatter(X[i,0], X[i,1], color=c, marker=m)

    x1 = np.linspace(X[:,0].min(), X[:,0].max(), 100)
    x2 = -(w[0] + w[1]*x1) / w[2]
    plt.plot(x1, x2, 'g')

    plt.grid(); plt.xlabel("F1"); plt.ylabel("F2")
    plt.title("Decision Boundary")
    plt.show()

def perceptron(X, y, lr=0.01, epochs=1000):
    w = np.zeros(X.shape[1])
    b = 0
    for _ in range(epochs):
        for i, x in enumerate(X):
            y_pred = np.sign(np.dot(x, w) + b)
            if y_pred != y[i]:
                u = lr * y[i]
                w += u * x
                b += u
    return w, b

X = np.array([[2,3],[3,4],[4,5],[5,6],[3,8],[2,7],[2,5],[1,7]])
y = np.array([1,1,1,1,-1,-1,-1,-1])

w, b = perceptron(X, y)
print("Weights:", w, "Bias:", b)

plot_data(X, y, np.insert(w, 0, b))

```

---

# Practical 8

## Membership and Identity Operators — `in`, `not in`

```python
a, b = 10, 20
lst = [1, 2, 3, 4, 5]

print("a in list" if a in lst else "a not in list")
print("b not in list" if b not in lst else "b in list")

a = 2
print("a in list" if a in lst else "a not in list")
```
## Membership and Identity Operators — `is`, `is not`

```python
a = [1, 2, 3]
b = a
c = [1, 2, 3]

print("a is b" if a is b else "a is not b")
print("a is not c" if a is not c else "a is c")

```
# practical 9

## Find ratios using fuzzy logic. 

```python
from fuzzywuzzy import fuzz

s1 = "I like softcomputing"
s2 = "I like hardcomputing"

print("Ratio:", fuzz.ratio(s1, s2))
print("Partial:", fuzz.partial_ratio(s1, s2))
print("TokenSort:", fuzz.token_sort_ratio(s1, s2))
print("TokenSet:", fuzz.token_set_ratio(s1, s2))
print("WRatio:", fuzz.WRatio(s1, s2))

```

## Solve Tipping problem using fuzzy logic 

```python
import numpy as np
import skfuzzy as fuzz
from skfuzzy import control as ctrl
import matplotlib.pyplot as plt

quality = ctrl.Antecedent(np.arange(0,11), 'quality')
service = ctrl.Antecedent(np.arange(0,11), 'service')
tip     = ctrl.Consequent(np.arange(0,26), 'tip')

for var in (quality, service):
    var['poor']     = fuzz.trimf(var.universe, [0,0,5])
    var['average']  = fuzz.trimf(var.universe, [0,5,10])
    var['excellent']= fuzz.trimf(var.universe, [5,10,10])

tip['low']    = fuzz.trimf(tip.universe, [0,0,13])
tip['medium'] = fuzz.trimf(tip.universe, [0,13,25])
tip['high']   = fuzz.trimf(tip.universe, [13,25,25])

rules = [
    ctrl.Rule(quality['poor'] | service['poor'], tip['low']),
    ctrl.Rule(service['average'], tip['medium']),
    ctrl.Rule(quality['excellent'] | service['excellent'], tip['high'])
]

sim = ctrl.ControlSystemSimulation(ctrl.ControlSystem(rules))
sim.input['quality'] = 6.5
sim.input['service'] = 9.8
sim.compute()

print("Recommended tip:", sim.output['tip'])

quality.view(); service.view(); tip.view()
plt.show()

```

