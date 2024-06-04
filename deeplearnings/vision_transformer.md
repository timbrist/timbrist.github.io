# Vision Transformer(ViT)

Origin paper at : https://arxiv.org/pdf/2010.11929

This page will serve as my note, building ViT from scratch. 

## split an image into fixed-size patches,


## Transformer Encoder 

![TE](https://timbrist.github.io/deeplearnings/transformer_encoder.png)

```Python
class TransformerEncoder(nn.Module):#import torch as nn
    def __init__(self, input_dim, hidden_dim, num_heads, dropout=0.0):
        super().__init__()
        self.norm = nn.LayerNorm(input_dim)
        self.multihead = nn.MultiheadAttention(input_dim, num_heads)
        self.fc = nn.Linear(input_dim, hidden_dim)
        # Multilayer Perception
        self.mlp = nn.Sequential(
            nn.Linear(d_model, d_model*r_mlp),
            nn.GELU(),
            nn.Linear(d_model*r_mlp, d_model)
        )

    def forward(self, embedded_patches):
        y1 = self.norm(embedded_patches)
        y2 = embedded_patches + self.multihead(y1,y1,y1)
        y3 = self.norm(y2)
        y4 = y2+self.mlp(y3)
        return y4
```