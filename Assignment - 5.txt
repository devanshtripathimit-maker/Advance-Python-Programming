def LCS(X, Y):
    m = len(X)
    n = len(Y)

    # Create LCS table
    lcs_table = [[0] * (n + 1) for _ in range(m + 1)]

    # Build the LCS table
    for i in range(m + 1):
        for j in range(n + 1):
            if i == 0 or j == 0:
                lcs_table[i][j] = 0

            elif X[i - 1] == Y[j - 1]:
                lcs_table[i][j] = lcs_table[i - 1][j - 1] + 1

            else:
                lcs_table[i][j] = max(
                    lcs_table[i - 1][j],
                    lcs_table[i][j - 1]
                )

    # Length of LCS
    index = lcs_table[m][n]

    # Array to store LCS string
    lcs_string = [''] * (index + 1)
    lcs_string[index] = '\0'

    i = m
    j = n

    # Backtracking to find LCS
    while i > 0 and j > 0:

        if X[i - 1] == Y[j - 1]:
            lcs_string[index - 1] = X[i - 1]
            i -= 1
            j -= 1
            index -= 1

        elif lcs_table[i - 1][j] > lcs_table[i][j - 1]:
            i -= 1

        else:
            j -= 1

    return ''.join(lcs_string[:-1])


# Example usage
X = "AGGTAB"
Y = "GXTXAYB"

print("Longest Common Subsequence:", LCS(X, Y))