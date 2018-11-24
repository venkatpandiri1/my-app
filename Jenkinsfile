
def testList = ["a", "b", "c", "d"]
def branches = [:] 
def NODE_NAME = "slave1"
def date
for (int i = 0; i < 4 ; i++) {
       int index=i, branch = i+1
       stage ("branch_${branch}"){ 
            branches["branch_${branch}"] = { 
                node ('maven-label'){
                       date = new Date()
                       println date
                    sh "echo 'node: ${NODE_NAME},  index: ${index}, i: ${i}, testListVal: " + testList[index] + "'"
                
                        writeFile file: "/tmp/branch_${branch}.txt", text: "This file is useful, need to archive it."
                }
            }
      }
}
