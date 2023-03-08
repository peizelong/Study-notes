# Vulkan中的Descriptor

### Descriptor布局描述

一个 `VkDescriptorSetLayoutBinding`用来描述一个Descriptor

多个这样的`VkDescriptorSetLayoutBinding`组合起来就成为了一个Descriptor Set,也就是一个`VkDescriptorSetLayout`

多个`VkDescriptorSetLayout`组合起来就成为了一个VkPipelineLayout

上面这些并没有持有资源，只是对布局的一种描述，比如在shader中绑定在那个set中的哪个binding

---

### 资源的分配

而一个真正持有资源的Descriptor需要通过 `VkDescriptorSetAllocateInfo`使用 `VkDescriptorSetLayout`的布局信息来从 `VkDescriptorPool`中分配

当一切都分配好后就可以通过 `VkWriteDescriptorSet`这个结构体来描述具体的属性
