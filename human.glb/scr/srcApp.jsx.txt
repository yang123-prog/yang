import { useState } from 'react'
import { Canvas } from '@react-three/fiber'
import { OrbitControls, useGLTF } from '@react-three/drei'

// 3D人体模型组件
function HumanModel({ showBone, showMuscle }) {
  const { scene } = useGLTF('/human.glb')

  // 根据图层开关控制模型部件显示隐藏
  scene.traverse((child) => {
    if (child.name.toLowerCase().includes('bone')) {
      child.visible = showBone
    }
    if (child.name.toLowerCase().includes('muscle')) {
      child.visible = showMuscle
    }
  })

  return <primitive object={scene} scale={1.2} />
}

export default function App() {
  const [showBone, setShowBone] = useState(false)
  const [showMuscle, setShowMuscle] = useState(false)

  return (
    <div style={{ width: '100vw', height: '100vh', margin: 0, display: 'flex', background: '#111827' }}>
      {/* 左侧控制面板 */}
      <div style={{
        width: 240,
        padding: 24,
        backgroundColor: '#1f2937',
        color: '#ffffff'
      }}>
        <h2 style={{ marginTop: 0, fontSize: 18 }}>解剖图层控制</h2>

        <div style={{ margin: 20px 0 }}>
          <label style={{ display: 'flex', gap: 8, alignItems: 'center' }}>
            <input
              type="checkbox"
              checked={showBone}
              onChange={(e) => setShowBone(e.target.checked)}
            />
            显示骨骼
          </label>
        </div>

        <div style={{ margin: 20px 0 }}>
          <label style={{ display: 'flex', gap: 8, alignItems: 'center' }}>
            <input
              type="checkbox"
              checked={showMuscle}
              onChange={(e) => setShowMuscle(e.target.checked)}
            />
            显示肌肉
          </label>
        </div>

        <div style={{ marginTop: 40, fontSize: 12, color: '#aaa' }}>
          <p>🖱 左键拖拽：旋转模型</p>
          <p>🖱 滚轮：放大缩小</p>
          <p>🖱 右键拖拽：平移画面</p>
        </div>
      </div>

      {/* 右侧3D渲染画布区域 */}
      <div style={{ flex: 1 }}>
        <Canvas camera={{ position: [0, 1.6, 4] }}>
          {/* 环境灯光 */}
          <ambientLight intensity={0.6} />
          <directionalLight position={[4, 4, 4]} intensity={0.8} />

          <HumanModel showBone={showBone} showMuscle={showMuscle} />

          {/* 视角控制器 */}
          <OrbitControls
            enableRotate
            enableZoom
            enablePan
            minDistance={2}
            maxDistance={10}
          />
        </Canvas>
      </div>
    </div>
  )
}