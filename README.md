# CRUD in Django 1.8

```python
from django.shortcuts import render, get_object_or_404
from django.shortcuts import redirect
from django.utils import timezone
from .models import Post
from .forms import PostForm

# Create your views here.

def index(request):
    posts = Post.objects.order_by('-id')
    return render(request, 'blog/post_list.html', {'posts':posts})

def show(request, pk):
    post = get_object_or_404(Post, pk=pk)
    return render(request, 'blog/post_detail.html', {'post': post})

def create(request):
    if request.method == 'POST':
        form = PostForm(request.POST or None)
        if form.is_valid():
            post = form.save(commit=False)
            post.author = request.user
            post.save()
            return redirect('blog.views.show', pk=post.pk)
    else:
         form = PostForm()
    return render(request, 'blog/post_edit.html',{'form':form})

def edit(request, pk):
    post = get_object_or_404(Post, pk=pk)
    if request.method == 'POST':
        form = PostForm(request.POST, instance=post)
        if form.is_valid():
            post = form.save(commit=False)
            post.author = request.user
            post.save()
            return redirect('blog.views.show', pk=post.pk)
    else:
        form = PostForm(instance=post)
    return render(request, 'blog/post_edit.html', {'form': form})

def destroy(request, pk):
    post = get_object_or_404(Post, pk=pk)
    post.delete()
    return redirect('blog.views.index')
```
