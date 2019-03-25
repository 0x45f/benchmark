�����ǻ���https://hub.docker.com/r/pytorch/pytorch/tags �ϵ� 0.4.1-cuda9-cudnn7-runtime������


1.���°�װconda 

mkdir conda
cd conda

apt-get install wget
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
sh Miniconda3-latest-Linux-x86_64.sh


2. ׼������

conda create --name maskrcnn_benchmark
conda activate maskrcnn_benchmark

conda install ipython

pip install ninja yacs cython matplotlib tqdm

conda install -c pytorch pytorch-nightly torchvision cudatoolkit=9.0

���ԣ�https://github.com/facebookresearch/maskrcnn-benchmark/blob/master/INSTALL.md

3. ��װcopoapi
���ﰲװ�ڣ�/ssd1/ljh/benchmark/Mask-RCNN/mask-rcc-fb_workspace
  cd benchmark/Mask-RCNN/mask-rcc-fb_workspace
  git clone https://github.com/cocodataset/cocoapi.git
  cd cocoapi/PythonAPI
  python setup.py build_ext install

4.����CUDA_HOME
find / -name "libnvblas.so.*"
�ҵ���·���磺 /home/work/docker/devicemapper/mnt/294816e447998c28557c2bca88ff5ee1573dac5dd6f51e0b875ea463ceeeaec2/rootfs/usr/local/cuda-9.0/
��Ϊ�ǰ�װ����conda���������һ����·��
ln -s /home/work/docker/devicemapper/mnt/294816e447998c28557c2bca88ff5ee1573dac5dd6f51e0b875ea463ceeeaec2/rootfs/usr/local/cuda-9.0/ /usr/local/cuda-9.0/

���뵽��������export CUDA_HOME=/usr/local/cuda-9.0/


4. ����maskrcnn-benchmark
  cd ../../maskrcnn-from-fb
  python setup.py build develop
  
����coco���ݵ�ַ
mkdir -p datasets/coco
ln -s /ssd1/ljh/dataset/COCO17/annotations datasets/coco/annotations
ln -s /ssd1/ljh/dataset/COCO17/train2017 datasets/coco/train2017
ln -s /ssd1/ljh/dataset/COCO17/test2017 datasets/coco/test2017
ln -s /ssd1/ljh/dataset/COCO17/val2017 datasets/coco/val2017


5. �޸�configs/e2e_mask_rcnn_R_50_FPN_1x.yaml

 coco2017�
  5.1 coco_2014_valminusminival ��Ϊ coco_2017_train

  5.2 �� SIZE_DIVISIBILITY: 32 ������һ�У�
      NUM_WORKERS: 0
6. ִ�����
  python tools/train_net.py --config-file "configs/e2e_mask_rcnn_R_50_FPN_1x.yaml" SOLVER.IMS_PER_BATCH 2 SOLVER.BASE_LR 0.0025 SOLVER.MAX_ITER 720000 SOLVER.STEPS "(480000, 640000)" TEST.IMS_PER_BATCH 1
 



