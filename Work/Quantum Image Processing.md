https://en.wikipedia.org/wiki/Quantum_image_processing#:~:text=By%20encoding%20and,computational%20basis%20states

"By [encoding](https://en.wikipedia.org/wiki/Encoding "Encoding") and processing the image information in [quantum-mechanical](https://en.wikipedia.org/wiki/Quantum_mechanics "Quantum mechanics") systems, a framework of quantum image processing is presented, where a pure [quantum state](https://en.wikipedia.org/wiki/Quantum_state "Quantum state") encodes the image information: to encode the [pixel](https://en.wikipedia.org/wiki/Pixel "Pixel") values in the probability amplitudes and the pixel positions in the computational basis states." 

Encode *pixel position* in computational **basis state**
Encode *pixel values* in **probability amplitudes**

A suitable encoding for binary data is the so-called [`BasisEmbedding`](https://pennylane.readthedocs.io/en/latest/code/api/pennylane.BasisEmbedding.html). The [`BasisEmbedding`](https://pennylane.readthedocs.io/en/latest/code/api/pennylane.BasisEmbedding.html) class interprets a binary string as a qubit basis state with the following mapping:
$$
\lvert 1 \rangle {\otimes} \lvert 1 \rangle \otimes \lvert 1 \rangle = \begin{bmatrix}0 \\ 1\end{bmatrix}\otimes\begin{bmatrix}0 \\ 1\end{bmatrix}\otimes\begin{bmatrix}0 \\ 1\end{bmatrix}=\begin{bmatrix}0 & 0 & 0 & 0 & 0 & 0 & 0 & 1\end{bmatrix}^{\top}$$
See below for a simple example of the [`BasisEmbedding`](https://pennylane.readthedocs.io/en/latest/code/api/pennylane.BasisEmbedding.html) used to initialize three qubits.

```python
import pennylane as qml

N = 3
wires = range(N)
dev = qml.device("default.qubit", wires)

@qml.qnode(dev)
def circuit(b):
    qml.BasisEmbedding(b, wires)
    return qml.state()
>>> circuit([1, 1, 1])
[0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 1.+0.j]
```

As expected, the result corresponds to the state |111⟩=|1⟩⊗|1⟩⊗|1⟩|111⟩=|1⟩⊗|1⟩⊗|1⟩. Representing the |1⟩|1⟩ state as the second standard basis vector and the tensor product as the [Kronecker product](https://en.wikipedia.org/wiki/Kronecker_product), we can confirm the result with a quick calculation.

|1⟩⊗|1⟩⊗|1⟩=[01]⊗[01]⊗[01]=[00000001]⊤|1⟩⊗|1⟩⊗|1⟩=[01]⊗[01]⊗[01]=[00000001]⊤

You can also just pass an integer value to the basis embedding function and it will automatically convert it into its binary representation. We can perform a quick sanity check of this functionality by running

```python
>>> print(circuit(7))
[0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 1.+0.j]
```

which is the same state vector we saw before. Unsurprisingly, the binary label corresponding to this state vector is also consistent with the binary representation of the integer seven.


[Quantum Edge Detection](https://arxiv.org/pdf/1801.01465.pdf)

This suggests that number of pixels must correspond to the number of qubits, about $log_{2}(n)$ pixels 
