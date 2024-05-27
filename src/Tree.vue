<template>
  <div>
    <el-radio-group v-model="radio1" size="large" @change="handchange">
      <el-radio-button label="New York" value="New York" />
      <el-radio-button label="Washington" value="Washington" />
      <el-radio-button label="Los Angeles" value="Los Angeles" />
      <el-radio-button label="Chicago" value="Chicago" />
    </el-radio-group>
  </div>
  <el-input v-model="filterText" style="width: 240px" placeholder="Filter keyword" />

  <el-tree :key="treekey" ref="treeRef" show-checkbox style="max-width: 600px" class="filter-tree" :data="data"
    :props="defaultProps" node-key="id" :default-expanded-keys="expendKeys" :filter-node-method="filterNode"
    @check="checkChange" />
  <Button @click="del1">删除</Button>
  <el-table :data="tableData" style="width: 100%">
    <el-table-column prop="date" label="Date" width="180" />
    <el-table-column prop="name" label="Name" width="180" />
    <el-table-column prop="address" label="Address" />
  </el-table>
</template>

<script lang="ts" setup>
import { ref, watch } from "vue";
import { ElTree } from "element-plus";
const radio1 = ref("New York");
const treekey = ref(new Date());
const expendKeys = ref([]);
const tableData =ref([]) 
interface Tree {
  [key: string]: any;
}

const handchange = (value: any) => {
  
  if (value == "New York") {
    treekey.value = new Date();
    expendKeys.value = getSecondIds("1");
  } else if (value === "Washington") {
    treekey.value = new Date();
    console.log(getSecondIds("2"));
    expendKeys.value = getSecondIds("2");
  } else if (value === "Los Angeles") {
    treekey.value = new Date();
    expendKeys.value = getSecondIds("3");
  }
  console.log(expendKeys.value);
  
};
const filterText = ref("");
const treeRef = ref<InstanceType<typeof ElTree>>();

const del1 = ()=>{
  console.log(treeRef.value!.getCheckedKeys(true)); //keys
  const keys = treeRef.value!.getCheckedKeys(true);
   const newkeys = keys.filter(item=> item !== 9)
  const newkeysCopy = [...newkeys];
  
  console.log(newkeysCopy);
  
  treeRef.value.setCheckedKeys([],false)
  console.log(treeRef.value!.getCheckedKeys(false)); //keys

   treeRef.value.setCheckedKeys(newkeysCopy, true);
   console.log(treeRef.value!.getCheckedKeys(true)); //keys
 
 
}
const defaultProps = {
  children: "children",
  label: "label",
};
// const getCheckedKeys = () => {
//   console.log(treeRef.value!.getCheckedKeys(false));
// };
// const checkChange = (node, date) => {
//   console.log("node", date.checkedNodes);
// };
watch(filterText, (val) => {
  treeRef.value!.filter(val);
});
const filterNode = (value: string, data: Tree) => {
  if (!value) return true;
  return data.label.includes(value);
};

const checkChange = (data1, node) => {
  node.checkedNodes.forEach((item) => {
    if (item.level === 3) {
      data.map((item1) => {
        if (item1.children && item1.pid === item.pid) {
          item1.children.map((item2) => {
            if (item2.children && item2.children.length > 0) {
              item2.children.map((item3) => {
                if (item3.label === item.label) {
                  tableData.value.push({
                    date: item1.label,
                    name: item2.label,
                    address: item.label,
                  });
                }
              });
            }
          });
        }
      });
    } else if (item.level === 2) {
      data.map((item1) => {
        if (item1.children && item1.pid === item.pid) {
          item1.children.map((item2) => {
            if (!item2.children || item2.children.length === 0) {
              tableData.value.push({ date: item1.label, name: item2.label,address:item.label });
            }
          });
        }
      });
    } else if (item.level === 1) {
      data.map((item1) => {
        if (item1.pid === item.pid) {
          if (!item1.children || item1.children.length === 0) {
            tableData.value.push({ date: item1.label, name: item1.label,address:item.label  });
          }
        }
      });
    }
  });
  console.log("tableData", tableData.value);
};
const data: Tree[] = [
  {
    id: 1,
    label: "Level one 1",
    level: "1",
    children: [
      {
        id: 4,
        label: "Level two 1-1",
        level: "2",
        children: [
          {
            id: 9,
            label: "Level three 1-1-1",
            level: "3",
          },
          {
            id: 10,
            label: "Level three 1-1-2",
            level: "3",
          },
        ],
      },
    ],
  },
  {
    id: 2,
    label: "Level one 2",
    level: "1",
    children: [
      {
        id: 5,
        label: "Level two 2-1",
        level: "2",
      },
      {
        id: 6,
        label: "Level two 2-2",
        level: "2",
      },
    ],
  },
  {
    id: 3,
    label: "Level one 3",
    level: "1",
    children: [
      {
        id: 7,
        label: "Level two 3-1",
        level: "2",
      },
      {
        id: 8,
        label: "Level two 3-2",
        level: "2",
      },
    ],
  },
];
const getSecondIds = (id) => {
  const ids = [];
  data.forEach((item) => {
    // item.children && item.children.length > 0 &&
    if ( id === "2") {
      ids.push(item.id);
      // item.children.forEach((item1) => {
      //   ids.push(item1.id);
      // });
      // item.children && item.children.length > 0 && 
    } else if (id === "3") {
      item.children.forEach((item1) => {
        item.children.forEach((item1) => {
        ids.push(item1.id);
      });
        // if (item1.children && item1.children.length > 0) {
        //   item1.forEach((item2) => {
        //     ids.push(item2.ids);
        //   });
        // }
      });
    }else if(id=="1"){
        
    }
  });
  return ids;
};

// [
//   {
//     date: "2016-05-03",
//     name: "Tom",
//     address: "No. 189, Grove St, Los Angeles",
//   },
//   {
//     date: "2016-05-02",
//     name: "Tom",
//     address: "No. 189, Grove St, Los Angeles",
//   },
//   {
//     date: "2016-05-04",
//     name: "Tom",
//     address: "No. 189, Grove St, Los Angeles",
//   },
//   {
//     date: "2016-05-01",
//     name: "Tom",
//     address: "No. 189, Grove St, Los Angeles",
//   },
// ];
</script>
