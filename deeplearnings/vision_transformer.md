# Vision Transformer(ViT)

Origin paper at : https://arxiv.org/pdf/2010.11929

This page will serve as my note, building ViT from scratch. 
![ViT](https://timbrist.github.io/deeplearnings/vision_transformer.png)

## Patch + Position Embedding

Reshape image \( x \in \mathbb{R}^{ \mathbf{H} \times \mathbf{W} \times \mathbf{C} }\) into a sequence of flattened 2D patches \( x_p \in \mathbb{R}^{ \mathbf{N} \times (\mathbf{P}^2 \cdot \mathbf{C})  } \), where \(N = HW/P^2\).


```Python
class PatchEmbed(nn.Module):
  def __init__(self, patch_size, in_channels, embed_dim):
    super().__init__()
    self.patch_conv = nn.Conv2d(in_channels, embed_dim, kernel_size=patch_size, stride=patch_size)
  def forward(self, x):
    # Patchify the image
    patches = x.unfold(dimension=2, size=patch_size, stride=patch_size).unfold(dimension=3, size=patch_size, stride=patch_size)
    # Reshape patches and apply linear projection
    return self.patch_conv(patches).reshape(x.shape[0], -1, embed_dim)

class PositionalEncoding(nn.Module):
  def __init__(self, num_patches, embed_dim):
    super().__init__()
    self.position_embeddings = nn.Parameter(torch.randn(num_patches+1, embed_dim)) # +1 for class token

  def forward(self):
    return self.position_embeddings
```



## Transformer Encoder 

![TE](https://timbrist.github.io/deeplearnings/transformer_encoder.png)

```Python
class TransformerEncoder(nn.Module):#import torch as nn
    def __init__(self, input_dim, hidden_dim, num_heads):
        super().__init__()
        self.norm = nn.LayerNorm(input_dim)
        self.multihead = nn.MultiheadAttention(input_dim, num_heads)
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

## ViT

```Python
class VisionTransformer(nn.Module):
    def __init__(self, input_dim, hidden_dim, num_heads):
        super().__init__()
        self.norm = nn.LayerNorm(input_dim)
        self.multihead = nn.MultiheadAttention(input_dim, num_heads)
        # Classification MLP
        self.mlphead = nn.Sequential(
            nn.Linear(self.d_model, self.n_classes),
            nn.Softmax(dim=-1)
        )
    def forward(self, embedded_patches):
        x = self.patch_embedding(images)
        x = self.positional_encoding(x)
        x = self.transformer_encoder(x)
        x = self.mlphead(x[:,0])
        return x
```

