# Huffman-Shannon_fano
# Aim:
Consider a discrete memoryless source with symbols and statistics {0.125, 0.0625, 0.25, 0.0625, 0.125, 0.125, 0.25} for its output. 
Apply the Huffman and Shannon-Fano to this source. 
Show that by drawing the tree diagram, and 
Calculate the average code word length, entropy, variance, redundancy, and efficiency.
# Tools Required:
google colab
# Program:
```
import heapq
import math

# -------------------------------------------------
# Source Probabilities
# -------------------------------------------------
symbols = ['S1','S2','S3','S4','S5','S6','S7']
probabilities = [0.125, 0.0625, 0.25, 0.0625, 0.125, 0.125, 0.25]

source = dict(zip(symbols, probabilities))

# -------------------------------------------------
# 1. HUFFMAN CODING
# -------------------------------------------------

class Node:
    def __init__(self, symbol, prob):
        self.symbol = symbol
        self.prob = prob
        self.left = None
        self.right = None

    def __lt__(self, other):
        return self.prob < other.prob


def build_huffman_tree(source):
    heap = [Node(sym, prob) for sym, prob in source.items()]
    heapq.heapify(heap)

    while len(heap) > 1:
        node1 = heapq.heappop(heap)
        node2 = heapq.heappop(heap)

        merged = Node(None, node1.prob + node2.prob)
        merged.left = node1
        merged.right = node2

        heapq.heappush(heap, merged)

    return heap[0]


def generate_huffman_codes(node, prefix="", codebook={}):
    if node:
        if node.symbol is not None:
            codebook[node.symbol] = prefix
        generate_huffman_codes(node.left, prefix + "0", codebook)
        generate_huffman_codes(node.right, prefix + "1", codebook)
    return codebook


huffman_tree = build_huffman_tree(source)
huffman_codes = generate_huffman_codes(huffman_tree)

# -------------------------------------------------
# 2. SHANNON-FANO CODING
# -------------------------------------------------

def shannon_fano(symbols_probs, codebook, prefix=""):
    if len(symbols_probs) == 1:
        symbol = symbols_probs[0][0]
        codebook[symbol] = prefix
        return

    symbols_probs.sort(key=lambda x: x[1], reverse=True)

    total = sum([x[1] for x in symbols_probs])
    acc = 0
    split_index = 0

    for i in range(len(symbols_probs)):
        acc += symbols_probs[i][1]
        if acc >= total / 2:
            split_index = i
            break

    left = symbols_probs[:split_index+1]
    right = symbols_probs[split_index+1:]

    shannon_fano(left, codebook, prefix + "0")
    shannon_fano(right, codebook, prefix + "1")


shannon_codes = {}
symbols_probs = list(source.items())
shannon_fano(symbols_probs, shannon_codes)

# -------------------------------------------------
# 3. PERFORMANCE CALCULATIONS
# -------------------------------------------------

def calculate_metrics(source, codebook):

    # Average Code Length
    avg_length = sum(source[sym] * len(codebook[sym]) for sym in source)

    # Entropy
    entropy = -sum(p * math.log2(p) for p in source.values())

    # Variance
    variance = sum(source[sym] * (len(codebook[sym]) - avg_length)**2 for sym in source)

    # Redundancy
    redundancy = avg_length - entropy

    # Efficiency
    efficiency = entropy / avg_length

    return avg_length, entropy, variance, redundancy, efficiency


# Calculate for Huffman
h_avg, h_ent, h_var, h_red, h_eff = calculate_metrics(source, huffman_codes)

# Calculate for Shannon-Fano
s_avg, s_ent, s_var, s_red, s_eff = calculate_metrics(source, shannon_codes)

# -------------------------------------------------
# 4. DISPLAY RESULTS
# -------------------------------------------------

print("====== HUFFMAN CODING ======")
for sym in sorted(huffman_codes):
    print(f"{sym}: {huffman_codes[sym]}")
print("\nAverage Length:", round(h_avg,4))
print("Entropy:", round(h_ent,4))
print("Variance:", round(h_var,4))
print("Redundancy:", round(h_red,4))
print("Efficiency:", round(h_eff*100,2), "%")

print("\n====== SHANNON-FANO CODING ======")
for sym in sorted(shannon_codes):
    print(f"{sym}: {shannon_codes[sym]}")
print("\nAverage Length:", round(s_avg,4))
print("Entropy:", round(s_ent,4))
print("Variance:", round(s_var,4))
print("Redundancy:", round(s_red,4))
print("Efficiency:", round(s_eff*100,2), "%")

```
# Calculation:
<img width="555" height="727" alt="image" src="https://github.com/user-attachments/assets/1be93d0e-3c46-48d7-b245-2c8b9c3814cd" />
<img width="407" height="195" alt="image" src="https://github.com/user-attachments/assets/59b4f28b-e3e8-496f-8a96-2cd56a60c5c5" />
<img width="513" height="349" alt="image" src="https://github.com/user-attachments/assets/03d1faf6-193a-4605-b681-ec6e8208ad54" />

# Output
<img width="505" height="626" alt="image" src="https://github.com/user-attachments/assets/9e78c08a-bad7-4679-b4f4-379119c8d6c0" />

# Results:
the Huffman and Shannon-Fano to this source. 
Show that by drawing the tree diagram, and 
the average code word length, entropy, variance, redundancy, and efficiency is calculated.
